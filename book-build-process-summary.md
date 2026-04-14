# Book Build Process Summary: Lessons for a Production Multi-Agent Harness

## Context

Over 4 runs across 2 days, GLM 5.1 (running via `fcoder` on a 202k-token context window) built a 300,734-word technical book about Claude Code's source code. The final successful run (Run 4) took **14 hours 22 minutes**, dispatched **~175 subagents**, consumed **416.8 million tokens**, and ended with the parent at **60% context utilization** (never hitting the 95% autocompact threshold).

The process was orchestrated by a single thin-dispatcher parent, guided by a ~2,450-line implementation plan, using 27 templates (16 worker + 11 step-runner), a 12-check verification gate, manifest-driven state tracking, and append-only convergence logging. A separate monitoring session (Opus 4.6) watched the build, diagnosed issues in real-time, and applied plan fixes between runs.

This summary extracts the patterns that generalize to any production multi-agent self-orchestrating harness.

---

## 0. The Defining Insight: Self-Correction Is the Primary Activity

The single most important finding from this build: **54% of all dispatches (95 out of 175) were self-correction, not first-pass creation.** Step 4 (revision) alone accounted for 83 dispatches — 47% of the entire build. The "happy path" (initial writing, bootstrapping, assembly) was only 46% of the work.

This is not a failure of the system. It is the design. The 97.3% citation accuracy, 0 forbidden tokens, and structural integrity of the final book were achieved through iterative self-correction loops, not first-pass quality.

### Dispatch Breakdown

| Phase | Dispatches | % | Nature |
|-------|-----------|---|--------|
| Bootstrap (Steps 0, 0.5, 0.9) | 5 | 3% | Setup |
| First-pass writing (Step 1: 15 batches + 2 retries) | 17 | 10% | Creation |
| Front matter + intros (Step 2) | 1 | 1% | Creation |
| Initial audits (Step 3: 57 chapters) | 57 | 33% | Evaluation |
| **Revisions (Step 4: 83 dispatches)** | **83** | **47%** | **Self-correction** |
| Critical review (Step 5: 3 rounds) | 3 | 2% | Evaluation |
| Stitch audit + fixes (Step 6: 3 rounds + 9 fixes) | 12 | 7% | Self-correction |
| Back matter (Step 7) | 1 | 1% | Creation |
| Final assembly (Step 8: 2 rounds + bulk fix) | 3 | 2% | Self-correction |
| **Total** | **~175** | | |

### Audit Pass Accumulation (534 Total)

Each chapter averaged 9.4 audit passes across 7 audit types. By type:

| Audit Type | Total Passes | Avg per Chapter | Re-audit Rate |
|------------|-------------|-----------------|---------------|
| Accuracy | 108 | 1.89 | 89% of chapters re-audited |
| Polish | 108 | 1.89 | 89% |
| Gaps | 96 | 1.68 | 68% |
| Diagrams | 76 | 1.33 | 33% |
| Consistency | 73 | 1.28 | 28% |
| Cross-ref | 67 | 1.18 | 18% |
| Critical | 6 | 0.11 | (Step 5 only) |

The most-reworked chapters:
- Ch 36 (Hook Schema): 17 audit passes — nearly half the theoretical maximum of 35
- Ch 50 (Analytics/Cost): 16 passes — hit the 5-pass hard cap on accuracy AND polish
- Ch 1 (Big Picture): 15 passes
- Ch 4 (Runtime Stack): 15 passes

### What This Means for Harness Design

1. **Plan for correction to be the majority of the work.** A harness that only plans for first-pass execution is planning for ~46% of the actual workload. The remaining 54% is where quality is achieved.

2. **The revision step is the bottleneck, not the creation step.** Step 1 (writing) took 3.5 hours. Step 4 (revision) took 3+ hours with 83 dispatches. Step 3 (auditing) took 3 hours. The creation phase is one-third of the pipeline; the evaluate-correct loop is two-thirds.

3. **Convergence is iterative, not binary.** No chapter passed all 6 audits on the first try. Every chapter required at least one revision. 89% of chapters needed accuracy re-audits. The system's value is its ability to systematically identify and fix issues, not to produce correct output on the first attempt.

4. **Hard caps are essential because self-correction loops can diverge.** Ch 50 hit the 5-pass hard cap on two audit types. Without caps, the audit-revise cycle would have continued indefinitely, chasing diminishing improvements. The cap forces a decision: accept current quality or escalate.

5. **The audit taxonomy determines correction quality.** The 6 auditor types (accuracy, cross-ref, diagrams, polish, consistency, gaps) each caught different issues. Accuracy and polish had the highest re-audit rates (89%), meaning they were the hardest to satisfy. A harness with only one quality check would miss the dimensional nature of quality.

---

## 1. What Works Well

### 1.1 Three-Tier Hierarchical Dispatch

The single most important architectural decision. Three roles with strict separation:

| Role | Reads | Writes | Context Budget |
|------|-------|--------|----------------|
| **Parent** (thin dispatcher) | manifest.json, parent-state.md, verdict JSONs, convergence.log tail | Nothing (only dispatches agents) | <40% of window |
| **Step-Runner** (phase coordinator) | Its template, manifest, relevant inputs | Manifest updates, convergence events, worker dispatch | Full window per agent |
| **Worker** (single-use) | Its template, chapter brief, source files, HER excerpt | One output file + one STATUS line | Full window per agent |

Why it works: The parent never touches content. It dispatches step-runners, reads their one-line STATUS return, updates its mental model, and dispatches the next step. This is what kept the parent at 60% after 175 dispatches over 14 hours. Run 1 failed precisely because it lacked this hierarchy — the parent tried to read source files and chapter content directly, bloating to >95% by chapter 34.

**Extractable pattern**: Any harness doing long-running multi-step work should have a thin orchestrator that only reads structured status (JSON verdicts, manifests) and delegates all content-touching work to disposable subagents.

### 1.2 Verification Gate as a Hard Halt

Step 0.9 runs 12 mandatory checks before any content-producing work begins. If any check fails, the build halts — no graceful degradation, no "we'll fix it later."

The checks validate structural prerequisites: Do all 27 templates exist? Are all 57 HER excerpts present? Does parent-state.md have enough content? Do all source file paths in the manifest resolve to real files on disk?

Why it works: Run 2 skipped this gate entirely, and the result was 57 chapters written against a broken build state (missing templates, incomplete parent-state). The gate would have caught both issues in under 3 minutes. The cost of a 3-minute gate check is trivial compared to the cost of restarting a 14-hour build.

**Extractable pattern**: Before entering the expensive phase of any multi-agent pipeline, run a deterministic structural validation that halts on failure. The gate should check: Are all inputs present? Are they well-formed? Do referenced resources exist? Is the state machine in the expected state? Fast-fail is cheaper than drift-and-crash.

### 1.3 Template System (Instructions on Disk, Not in Context)

The 27 templates were extracted once (Step 0.5) and stored as `.md` files in `.book-build/templates/`. Every subagent reads its template from disk at dispatch time. The parent never re-embeds template content in dispatch prompts.

Why it works: If templates were inlined in the parent's dispatch messages, each dispatch would add 2-5KB to parent context. Over 175 dispatches, that's 350-875KB of redundant template text — more than the entire context window. By storing templates on disk and having subagents self-load, the parent's dispatch message is just: "Read your template at path X, read the manifest for chapter Y, execute, return STATUS."

**Extractable pattern**: Any reusable instruction set (agent role definitions, quality criteria, output format specs) should be stored as files that subagents read on initialization. The dispatcher should pass paths, not content.

### 1.4 Manifest-Driven State Tracking

`manifest.json` is the single source of truth for the build's state. Every chapter has: id, status (pending -> drafted -> audited -> done), pass_counts (accuracy, crossref, diagrams, polish, consistency, gaps — max 5 each), word_count, citation counts, and verdict history.

Step-runners update the manifest atomically after each worker completes. The parent reads the manifest to decide what to dispatch next.

Why it works: JSON state is deterministic, inspectable, and survives context compaction. The parent can be compacted to 10% context and still know exactly where the build stands by reading manifest.json. Human operators can also inspect the manifest at any time to understand progress.

**Extractable pattern**: Multi-agent state should live in a structured file (JSON/YAML), not in conversation history. Every agent writes its results to this file. The orchestrator reads it to make dispatch decisions. This decouples state from context.

### 1.5 Append-Only Convergence Logging

`convergence.log` is a JSONL file where every significant event is appended: bootstrap_done, templates_extracted, writer_success, audit verdicts, revision events, critical review rounds, stitch outcomes, final assembly results.

Why it works: It provides a complete audit trail that survives autocompaction. When the parent's context gets compressed, the log retains the full history. It also enabled the monitoring session (me) to diagnose issues by tailing the log. Post-mortem analysis of failed runs was possible because the log preserved the sequence of events.

**Extractable pattern**: Every multi-agent system should have an append-only event log. Events should be structured (JSON), timestamped, and include: step, agent type, chapter/task ID, outcome, and key metrics. The log is the system's memory — the orchestrator's context is not.

### 1.6 One-Line STATUS Contract

Every subagent returns exactly one line: `STATUS: {"status":"done"|"failed", "step":"N", ...}`. No prose before or after. The parent parses this one line and discards the rest.

Why it works: This is what keeps parent context from growing. If subagents returned multi-paragraph summaries, each dispatch would add ~500 tokens to the parent's conversation history. Over 175 dispatches, that's 87,500 tokens — 43% of the window consumed just by return messages. The one-line contract limits each return to ~50 tokens.

**Extractable pattern**: Subagent return values should be structured, minimal, and machine-parseable. Rich output (reports, prose, diagnostics) should be written to files on disk. The dispatch loop should only consume structured status.

### 1.7 Hard Caps with Escape Hatches

The system enforces absolute limits: 5 audit passes per reason per chapter, 3 rewrites per chapter, 4 critical review rounds, 3 stitch rounds, 10 total audit rounds. If limits are hit, the chapter escalates to "needs_critical_escalation" instead of looping forever.

Why it works: Without hard caps, audit-revision cycles can loop indefinitely — the auditor always finds something, the reviser fixes it but introduces something new, ad infinitum. The caps force convergence: after 5 attempts, either the chapter passes or it's escalated. The 10-round escape hatch ensures the build terminates even if individual chapters can't converge.

**Extractable pattern**: Every feedback loop in a multi-agent system needs a hard cap and an escalation path. The cap should be low enough to prevent cost explosion but high enough to allow genuine improvement. When the cap is hit, the system should not silently continue — it should escalate or halt.

---

## 2. What Helped the Success of the Book

### 2.1 Pre-Launch Plan Review (20 Fixes)

Before Run 4, a separate Opus session reviewed the entire 2,450-line plan and found **20 errors**: 5 critical (wrong repo stats, inflated LOC claims, missing exemptions), 10 inconsistencies (STATUS format mismatches, section range errors, parallelism cap disagreements), and 5 gaps (missing back-matter auditor template, unassigned terminology merging, missing prerequisites).

This review took ~2 hours but prevented the entire class of failures that killed Runs 1-3. Without it, Run 4 would have likely failed the same way.

**Lesson**: Never launch a complex multi-agent pipeline without an independent review of the plan/prompt. The plan is the system's firmware — errors in the plan propagate to every subagent.

### 2.2 Dedicated Monitoring Session

A separate Opus session (me) ran alongside GLM 5.1 for the entire 14 hours. This session:
- Watched parent context growth (flagged when it passed 40%)
- Ran independent verification commands (chapter counts, Chinese character scans, word counts)
- Diagnosed the Step 0.9 false positive (meta-chapters with empty source_files) and provided the fix
- Applied plan edits (C2 check exemption, C3 directory expansion rule, step-0-runner source_files rules) that GLM 5.1 picked up on re-dispatch
- Advised on option selection when GLM 5.1 paused for decisions (e.g., "select Option 1: re-dispatch step-0-runner")

**Lesson**: Long-running multi-agent builds benefit enormously from a monitoring agent with a different context. The build agent is deep in execution; the monitor has fresh perspective and can read artifacts the builder has already compacted away. This is the "generator-evaluator" pattern applied to the orchestration layer itself.

### 2.3 Clean Restarts Over Mid-Session Recovery

The user consistently chose to archive failed runs and restart from scratch rather than attempt mid-session recovery. This meant:
- Each run started with a clean `.book-build/` directory
- No corrupted state carried forward
- Plan fixes were incorporated from the beginning

Run 4 succeeded on the first attempt because all 20 plan fixes were active from Step 0. If we had tried to resume Run 3 mid-session, we would have had to patch the running system while it was executing — much higher risk of inconsistent state.

**Lesson**: For multi-agent pipelines under active development, clean restarts are safer than hot patches. Design the system to be restartable (deterministic from clean state), not resumable (requires reconstructing mid-run state). Resume protocols can be added once the pipeline is stable.

### 2.4 The Exhaustive Plan Document

The 2,450-line plan included: chapter table with all 57 entries, source file lists per chapter, canonical corrections (files to remove/rename/add), full templates for all 27 agents, schema definitions for manifest/convergence/terminology, quality criteria, convergence rules, 35 risk mitigations, and a verification protocol.

Why it helped: GLM 5.1 never had to improvise. Every step had a template. Every decision had a rule. Every edge case had a mitigation. The plan's exhaustiveness meant the parent could be thin — it didn't need to carry decision logic because the plan encoded it.

**Lesson**: The more complete the plan, the thinner the orchestrator. Investment in plan quality pays off multiplicatively — every subagent benefits from clear instructions.

### 2.5 Permission Bypass for Unattended Execution

Run 3 failed because `~/.claude/settings.json` had `defaultMode: "plan"` globally, causing constant permission prompts despite project-level `acceptEdits`. Run 4 launched with `--dangerously-skip-permissions`, enabling 14 hours of unattended execution.

**Lesson**: Automated multi-agent builds must run with pre-authorized permissions. The permission model should be configured once (with appropriate guardrails: deny rules for source code, build configs, destructive git operations) and then not interrupt execution. Interactive permission prompts are incompatible with autonomous orchestration.

### 2.6 Structural Enforcement of Gates (Not Just Prompts)

Fix C4 added a structural check to step-1-batch-runner: before dispatching any writers, it checks that `verification-0.9.md` exists and contains "pass". This is a file-existence check, not a prompt instruction. Run 2 failed because the gate was prompt-based only — the parent's prompt said "run Step 0.9 first" but nothing enforced it.

**Lesson**: Critical prerequisites should be enforced structurally (file existence, JSON field values, manifest status checks), not just instructionally. Prompts can be ignored or misinterpreted under context pressure. File checks cannot.

---

## 3. What Failed or Was Painful (Avoid in Harnesses)

### 3.1 Run 1: No Hierarchy (Context Death)

The first attempt had no step-runner layer. The parent dispatched writers directly, read their output, ran audits itself, and tried to track state in its own context. By chapter 34, the parent had consumed its entire context window reading chapter content and source files.

**Root cause**: Violating the thin-dispatcher principle. The parent was doing work instead of delegating.

**Harness lesson**: The orchestrator must NEVER read content. It reads structured status only. All content processing happens in disposable subagents with fresh context.

### 3.2 Run 2: Skipped Verification Gate (Silent Drift)

Run 2 extracted only 15/27 templates (all step-runner templates missing), had an undersized parent-state.md (604 words instead of ~1000), and never ran the Step 0.9 gate — yet proceeded to write all 57 chapters against this broken state. The convergence log showed 57 `writer_success` events, but the chapters were built on incomplete instructions.

**Root cause**: The gate was advisory, not structural. Nothing prevented Step 1 from running without Step 0.9 passing.

**Harness lesson**: Gates must be enforced by the downstream step, not just the upstream plan. Step 1 should refuse to execute unless Step 0.9's output file exists and contains "pass". This is defensive programming applied to multi-agent pipelines.

### 3.3 Run 4, Step 0.9: False Positive Failures

The gate failed on C2 (meta-chapters with intentionally empty `source_files`) and C3 (directory paths not expanded to actual files). Both required plan edits and a re-dispatch cycle, costing ~15 minutes and operator attention.

**Root cause**: The verification checks were written for the common case but didn't account for valid exceptions (synthesis chapters with no primary sources, directory paths that need expansion).

**Harness lesson**: Verification gates need an exemption mechanism. Checks should have an `exempt` list or conditional logic for known-valid exceptions. The alternative — fixing false positives at runtime — requires operator intervention, defeating the purpose of automation.

### 3.4 Manifest Status State Machine Gap

At Step 8, the final runner found that no chapters had status="audited" — they were stuck in "drafted", "revised", or "revising". Step 4 revision runners updated chapter content but never advanced the status to "audited". GLM 5.1 initially tried to re-audit all 57 chapters (dispatching step-3-chapter-runners again), but the audit-revision cycle wouldn't converge because auditors always find minor issues.

GLM 5.1 eventually recognized the pattern and did a bulk status promotion (`jq` update of all 57 chapters to "audited"), which unblocked Step 8. This worked but was a workaround, not a design.

**Root cause**: The state machine transitions between steps were implicit (expected behavior) rather than explicit (coded in the step-runner templates). The step-4-revision-runner template didn't include "after revision, set status to 'audited'" because the plan assumed the audit-revision cycle would naturally converge.

**Harness lesson**: State machine transitions must be explicit in every step-runner template. "After completing this step, set `status` to X" should be a mandatory instruction. Never rely on emergent convergence for state transitions.

### 3.5 Forbidden Token Whack-a-Mole in Stitch Rounds

During Step 6, the stitch auditor found "simply" in chapters 10, 21, 56. Fix agents removed them. Round 2 found a title mismatch in Chapter 28. Fixed. Round 3 found "simply" again in chapters 12 and 54 — revision agents had reintroduced the forbidden word while fixing other issues.

This consumed all 3 allowed stitch rounds chasing a single forbidden word, with no guarantee the fixes didn't re-introduce it elsewhere.

**Root cause**: Revision agents are generative — they can introduce new violations while fixing old ones. The forbidden-token check runs at the stitch level (cross-chapter), but fixes happen at the chapter level (chapter-specific). There's no guarantee a chapter-level fix doesn't reintroduce a cross-chapter violation.

**Harness lesson**: Quality checks should run at the same granularity as fixes. If you fix at the chapter level, check at the chapter level before promoting. The stitch auditor should only check cross-chapter concerns (numbering, TOC, forward references), not per-chapter concerns (forbidden tokens) that should have been caught by the chapter-level polish auditor.

### 3.6 Language Drift Under Load

Starting around hour 5, GLM 5.1's parent began emitting Chinese status messages ("批次 10 和 11 已完成" instead of "Batches 10 and 11 completed"). The chapter content (written by subagents) was not affected — a Chinese character scan confirmed only 6 legitimate characters (the Japanese word "日本語" in a language-normalization code example).

**Root cause**: The parent's system prompt did not include an explicit language instruction. Under heavy token load, the model reverted to its training distribution, which includes Chinese.

**Harness lesson**: Every orchestrator prompt and every subagent template must include an explicit language instruction: "All output must be in English." This is not implied by the conversation context. Under load, models drift toward their training distribution. Explicit > implicit.

### 3.7 Plan Errors Propagated to Every Subagent

The original plan had 20 errors (5 critical, 10 inconsistencies, 5 gaps). Every subagent that read the plan inherited these errors. The inflated LOC claims (useCanUseTool at "40,000 lines" instead of 200) would have caused writers to allocate disproportionate coverage to tiny files. The missing back-matter auditor template would have caused Step 7 audits to fail with schema mismatches.

**Root cause**: The plan was written by an earlier model session without systematic verification against the actual codebase.

**Harness lesson**: Plans should be verified against ground truth before execution. Automated checks (file existence, line counts, path validation) should run as part of plan validation, not during execution. The 20-fix review session was essentially a "plan linting" step that should be formalized.

---

## 4. What Could Be Optimized

### 4.1 Conservative Parallelism

The build dispatched only 2 step-runners at a time for Steps 1 and 3 (the two most time-consuming phases). Step 1 took 3.5 hours for 57 chapters; Step 3 took 3 hours for 57 audits. With 4 concurrent step-runners (the plan's stated maximum for Step 4), these phases could potentially halve.

**Constraint**: The parent's context grows with each dispatch/return cycle (~0.15% per pair). Higher parallelism means faster completion but faster context growth. At 2-at-a-time, the parent hit 60% by build end. At 4-at-a-time, it might hit 70-75%, still well under the 95% autocompact threshold.

**Optimization**: Increase Step 1 and Step 3 parallelism to 4 concurrent step-runners. Monitor context growth and reduce if it exceeds 70% by Step 5.

### 4.2 Step 0.5 Context Fragility

Step 0.5 performs ~85 writes in a single agent session (27 templates + 57 HER excerpts + parent-state.md). Run 2 failed because this agent ran out of context. Fix C5 added a priority ordering (templates first, then excerpts, then parent-state), but the fundamental issue is that 85 writes in one session is fragile.

**Optimization**: Split Step 0.5 into sub-steps:
- Step 0.5a: Extract 27 templates (priority — these are structurally critical)
- Step 0.5b: Extract 57 HER excerpts (can be parallelized into batches of 10-15)
- Step 0.5c: Write parent-state.md

This adds 2 more dispatch cycles to the parent but eliminates the single-point-of-failure.

### 4.3 Tiered Re-Verification Instead of Full Re-Audit

The audit-revise loop is the core quality mechanism (see Section 0), not waste. However, re-running the full 6-auditor suite after every revision is heavier than necessary. After Step 4 (revision), the system re-ran Step 3 (full 6-auditor suite) on revised chapters, which found new minor issues (often introduced by the revision itself) and triggered another cycle.

The loop is correct in principle — but the granularity is wrong. When a revision fixes a citation error, re-running the polish auditor and gaps auditor is unnecessary. Only the accuracy auditor needs to re-check.

**Optimization**: Implement tiered re-verification:
- **Targeted re-check**: After a revision, only re-run the auditor types that flagged the issues being fixed. If accuracy flagged bad citations, only re-run accuracy — not all 6.
- **Full re-audit**: Run the complete 6-auditor suite only at milestone gates (end of Step 4, before Step 5 critical review).
- **Manifest flag**: Track which audit types are "stale" per chapter. A revision makes accuracy stale but not diagrams.

This would reduce the 534 total audit passes to perhaps 380-400 while preserving the same quality, cutting ~30% of the correction workload.

### 4.4 Step 8 Assembly Could Be Incremental

Step 8 stitches all 57 chapters + front matter + back matter into one 300k-word file. If it finds issues (e.g., forbidden tokens), it dispatches revision runners and then re-stitches the entire file. The re-stitch reads all 57 chapters again, even if only 2 changed.

**Optimization**: Maintain a "last-assembled" checksum per chapter. On re-stitch, only re-read chapters whose checksum changed. This would reduce Step 8 re-assembly from reading ~2.2MB to reading only the changed chapters.

### 4.5 Smarter Convergence Detection

The system uses "two consecutive Step 8 rounds with zero revisions" as the convergence criterion. But the first Step 8 round failed because of the manifest status state machine gap (not a content issue), and the second passed cleanly. A smarter system would distinguish between "content revisions needed" and "state machine bookkeeping needed."

**Optimization**: Separate structural checks (manifest status, file existence) from content checks (word count, citation accuracy, forbidden tokens). Structural issues should be fixed in-place without consuming a convergence round.

### 4.6 Checkpoint-Based Resume

All 4 runs required starting from scratch because there was no reliable resume mechanism. The plan includes a Section 13 resume protocol, but the user preferred clean restarts for safety. With a proper checkpoint system, Run 3 (which reached Step 3 before the permission issue) could have been resumed from Step 3 in Run 4, saving ~30 minutes.

**Optimization**: After each step completes, write a `checkpoint-N.json` with the build state (completed steps, manifest snapshot, convergence log hash). On restart, the system can detect a valid checkpoint and resume from the last completed step. The key constraint: the checkpoint must include enough state to reconstruct the parent's decision context without re-reading completed outputs.

---

## 5. Should a Dedicated Skill Replace the Prompt Approach?

### Current Approach: Prompt-Based

The entire orchestration lives in two files:
- `prompt.md` (~20KB): The "paste this into fcoder" launch prompt
- `book-implementation-plan.md` (~155KB): The plan that GLM 5.1 reads at startup

The parent reads ~175KB of instructions at Turn 1, consuming ~10% of its context window before doing any work. Every restart re-reads this. Every plan fix requires editing a 2,450-line document. There is no reusability — launching a different multi-agent task requires writing a new plan from scratch.

### Why a Skill Would Help

**A. Separation of mechanism from policy.**

The orchestration *mechanism* (dispatch loop, gate checks, STATUS parsing, manifest updates, convergence detection) is identical across tasks. The *policy* (57 chapters, 6 auditor types, 5 audit passes max, forbidden token list) is task-specific.

A skill would encode the mechanism once:
- Three-tier dispatch skeleton (parent -> step-runner -> worker)
- Verification gate framework (define checks, run all, halt on fail)
- Manifest schema and update protocol
- Convergence logging
- Hard cap enforcement
- One-line STATUS contract
- Template extraction and self-loading protocol
- Parallelism management (configurable concurrency limit)

The user supplies the policy:
- Task definition (what work needs to be done)
- Step sequence (what phases, in what order)
- Worker types (what subagent roles exist)
- Quality criteria (what checks, what thresholds)
- Convergence rule (what "done" means)

**B. Elimination of plan-reading context overhead.**

Instead of reading 155KB of plan at startup, the skill would provide the dispatch mechanism directly. The parent only reads the task-specific policy (chapter table, quality criteria) — perhaps 20-30KB instead of 155KB. This reclaims 8-10% of the context window.

**C. Built-in failure mode mitigations.**

Every lesson from this build process would be codified:
- Explicit language enforcement in all templates
- Structural gate checks (file existence, not just prompt instructions)
- Mandatory state-machine transitions in step-runner templates
- Checkpoint-based resume capability
- False-positive exemption mechanism in verification gates
- Separate per-item checks from cross-item checks in quality passes

**D. Reusability across projects.**

The same skill could orchestrate:
- A 57-chapter book (this project)
- A large-scale code refactoring (chapters = files, auditors = linters/tests)
- A research report with 20 sections (chapters = sections, writers = researchers)
- A documentation overhaul (chapters = docs, auditors = accuracy/completeness checkers)
- A multi-repository migration (chapters = repos, workers = migration scripts)

### What the Skill Should Look Like

```
/orchestrate
```

When invoked, the skill would:

1. **Brainstorm phase**: Ask the user what they want to build, how many units of work, what quality criteria, what "done" means.

2. **Plan generation**: Auto-generate a plan document with:
   - Task decomposition (units of work, dependencies, parallelism opportunities)
   - Step sequence (bootstrap -> validate -> produce -> audit -> revise -> review -> assemble)
   - Worker templates (one per role)
   - Step-runner templates (one per phase)
   - Verification gate checks
   - Convergence criteria

3. **Plan validation**: Run automated checks on the plan:
   - Do all referenced files exist?
   - Are all line counts accurate?
   - Are all template cross-references valid?
   - Do status values form a valid state machine?

4. **Execution**: Launch the thin-dispatcher loop:
   - Read manifest, decide next dispatch
   - Dispatch step-runner, parse STATUS
   - Update manifest, log convergence event
   - Check hard caps, check convergence
   - Repeat until done or halted

5. **Monitoring**: Provide real-time status to the user:
   - Current step, progress (N/M units done), context utilization
   - Alerts on anomalies (context > threshold, convergence stalled, hard cap hit)

### Skill vs. Plan Tradeoffs

| Aspect | Prompt/Plan Approach | Skill Approach |
|--------|---------------------|----------------|
| Flexibility | Maximum — plan can encode anything | Constrained to skill's dispatch model |
| Setup time | Hours (write plan + review) | Minutes (answer brainstorm questions) |
| Reusability | Zero (new plan per project) | High (same skill, different policy) |
| Error surface | Large (2,450 lines of natural language) | Small (mechanism is tested, only policy varies) |
| Context cost | ~10% for plan read | ~2% for policy read |
| Debugging | Read 2,450-line plan | Inspect skill logic + task-specific config |
| Evolution | Manual edits to plan | Version the skill, accumulate lessons |

**Recommendation**: Build the skill. The prompt/plan approach was necessary for exploration (discovering the right patterns through 4 iterations), but now that the patterns are known, they should be codified. The skill should be rigid on mechanism (dispatch loop, gates, convergence) and flexible on policy (task definition, quality criteria, worker roles).

---

## 6. Additional Observations

### 6.1 The Monitoring Agent Pattern Is Critical

The monitoring session (Opus 4.6) operated on a completely separate context from the build agent (GLM 5.1). This separation was invaluable:
- The monitor could read files the builder had compacted away
- The monitor could run verification commands without consuming builder context
- The monitor provided a "second opinion" on decisions (e.g., Option 1 vs Option 2 for Step 0.9 failures)
- The monitor applied plan fixes that the builder picked up on re-dispatch

This is the generator-evaluator pattern applied at the meta-level: the builder generates, the monitor evaluates. Future harnesses should formalize this with a dedicated monitoring agent that:
- Tails the convergence log
- Runs periodic health checks (context utilization, progress rate, anomaly detection)
- Alerts the user on red flags
- Can apply plan patches without interrupting the builder

### 6.2 Token Economics

| Metric | Value |
|--------|-------|
| Total session tokens | 416.8M |
| Final book size | 300,734 words (~400K tokens) |
| Token efficiency | ~1,040 tokens consumed per word of output |
| Subagent dispatches | ~175 |
| Average tokens per dispatch | ~2.4M |
| Parent context at completion | 60% (122K of 202K) |
| Build duration | 14h 22m |
| Failed runs before success | 3 |
| Plan review fixes | 20 |
| Real-time plan fixes during Run 4 | 3 (C2 exemption, C3 directory expansion, step-0-runner source_files rules) |

The 1,040:1 ratio reflects the cost of the audit-revise-review loop. A "first draft only" pipeline would be ~300:1, but the quality would be much lower (the review report confirmed 97.3% citation accuracy, which required multiple audit passes to achieve).

### 6.3 The Handoff Document Pattern

Between sessions, state was transferred via a `handoff.md` document — a structured briefing for the next session (monitoring assistant) covering: what happened, what files exist, the architecture, key milestones, red flags, verification protocol, and how to help.

This is an informal version of the session handoff mechanism that the book's own review identified as a critical gap in CC. Future harnesses should formalize this: when a session ends (cleanly or not), write a structured handoff file that enables a new session to understand the current state without re-reading all artifacts.

### 6.4 The Four Runs as an Iteration Cycle

| Run | Duration | Failure Point | Root Cause | Fix Category |
|-----|----------|---------------|------------|--------------|
| 1 | ~2h | Step 1 (Ch 34) | No hierarchy, parent read content | Architecture |
| 2 | ~3h | Step 1 (gate skipped) | Missing templates, no structural gate | Structural enforcement |
| 3 | ~45m | Step 1 (paused) | Permission prompts | Configuration |
| 4 | 14h 22m | **Completed** | (Plan review + permission bypass + gate fixes) | N/A |

Each run failed for a different reason, and each fix was a different type: Run 1 required an architecture change (add step-runner layer), Run 2 required structural enforcement (gate checks in downstream steps), Run 3 required configuration (permission bypass). This progression — architecture -> enforcement -> configuration -> success — is a natural maturation curve for any complex system.

**Lesson**: Expect 2-4 iterations before a multi-agent pipeline stabilizes. Design for fast failure and fast restart. The cost of 3 failed runs (~5 hours + operator time) was small compared to the value of the 20 plan fixes they motivated.

### 6.5 The Plan as the System's Firmware

The implementation plan is not documentation — it is the system's firmware. Every subagent reads sections of the plan to understand its role. Errors in the plan propagate to every subagent that reads the affected section. The 20-fix review was essentially a firmware QA pass.

Future harnesses should treat the plan/configuration with the same rigor as production code:
- Version it
- Review it before execution
- Validate it automatically (file paths exist, counts match, schemas are valid)
- Test it against a small subset before full execution

### 6.6 GLM 5.1 as an Orchestrator: Strengths and Limits

**Strengths observed**: GLM 5.1 followed the plan faithfully for 14 hours across 175 dispatches. It correctly identified the Step 0.9 false positive (meta-chapters), recognized the audit-revision convergence failure pattern and invented a workaround (bulk status promotion), and maintained context discipline (never exceeded 60%).

**Limits observed**: Language drift under load (Chinese status messages), inability to distinguish structural issues from content issues in convergence detection, and occasional re-reading of plan sections already in context (wasting tokens). The model also could not self-diagnose the permission prompt issue — that required operator intervention.

**Implication for harness design**: The orchestrator model should be optimized for reliability over capability. It doesn't need to be the most capable model — it needs to follow instructions precisely, manage state correctly, and fail gracefully. A smaller, faster model with strong instruction-following might actually be better than a large model with creative tendencies.
