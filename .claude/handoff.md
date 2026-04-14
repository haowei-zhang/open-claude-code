# Session Handoff: Book Fixes Complete, Ready for Harness Extraction

> **For the new Opus session**: Read this entire file. It contains everything you need. The book build was completed in Session 1. The book fixes (review report) were completed in Session 2 (this session). The next phase is extracting the orchestration patterns into a reusable multi-agent harness skill/system.

---

## 1. Who Is the User

- Name: Haowei Zhang
- Git user: Haowei Zhang
- Working directory: `/home/hwzhang/build/open-claude-code`
- Branch: `cc_research_book`
- Global CLAUDE.md rules: **Never** add `Co-Authored-By` lines or AI attribution to commits/PRs/MRs
- Workflow preference: **Prefers clean restarts** over mid-session recovery for unstable pipelines.

## 2. Session History

### Session 1: Book Build (14 hours, GLM 5.1 via fcoder)
Built a **300,734-word technical research book** ("Deep Research and Development Guide of CC Source Code") using hierarchical multi-agent orchestration. 57 chapters, 10 parts, 3 appendices. 4 runs (3 failures, 1 success). 416.8M tokens consumed. 97.3% citation accuracy. The book analyzes Claude Code's source code cross-referenced against a Harness Engineering Report (HER).

### Session 2: Book Fixes (this session, Opus 4.6)
Applied all review report fixes. The book grew from 29,388 to 32,164 lines (300,734 to 326,908 words). **All 23 of 24 review items addressed** (1 skipped: M-GAP-4 teleport, source dir doesn't exist).

## 3. What Session 2 Did (Complete Changelog)

### Phase 1: Bug Fixes (8/8 done)

| ID | Fix | Location |
|----|-----|----------|
| H-BUG-1 | 7 `#` → `//` in TypeScript code block citations | Ch 54 |
| M-BUG-1 | Preface section names updated to match actual chapters + line 15 paragraph | Preface |
| M-BUG-2 | "four compaction layers" → "five compaction stages" + added 5th stage to enumeration | Ch 53 |
| M-BUG-3 | Added inline note about duplicate `.describe()` being a source code artifact | Ch 53 |
| L-BUG-1 | `## Developer takeaways` → `## Developer takeaways for building a long-running agent` | Ch 28 |
| L-BUG-2 | Citation range `L57-L100` → `L57-L103` | Ch 53 |
| L-BUG-3 | 3 re-quoted code blocks (CompactionResult, COMPACTABLE_TOOLS, BashCommandHookSchema) replaced with cross-references to their canonical locations in Ch 28/36 | Ch 53, 54 |
| L-BUG-4 | Removed 3rd repetition of "24.9 percentage point spread" stat, replaced with back-reference | Ch 1 |

### Phase 2: High-Priority Content Additions (3/3 done)

| ID | Content | Lines Added | Location |
|----|---------|-------------|----------|
| H-GAP-1 | Build System / DCE: `feature()` gates, `require()` vs `import()`, 88-flag catalog table, DCE in tool registry + query loop, excluded-strings constraint, Mermaid flowchart | 354 lines | Ch 4 (before Developer takeaways) |
| H-GAP-2 | Authentication: OAuth 2.0 PKCE 8-step flow with sequenceDiagram, macOS Keychain + plaintext fallback, 8-source credential priority order, token refresh with 5-minute buffer, cloud provider credential chains (Bedrock/Vertex/Foundry) | 156 lines | Ch 8 (before Developer takeaways) |
| H-GAP-3 | Reference Appendices D-H: Tool Reference Table (50+ tools), System Prompt Section Catalog, Feature Flag Reference (97 flags), Hook Event Reference (28 events), Environment Variables (200+ vars) | 1,287 lines | After Appendix C |

### Phase 3: Medium-Priority Content Additions (5/6 done, 1 skipped)

| ID | Content | Location |
|----|---------|----------|
| M-GAP-1 | QueryGuard three-state machine (idle/dispatching/running), `useSyncExternalStore`, stale finally block prevention, stateDiagram-v2 | Ch 43 Control flow |
| M-GAP-2 | Speculative execution: overlay filesystem, `copyOverlayToMain()`/`safeRemoveOverlay()`, safety bounds (20 turns, 100 messages), CompletionBoundary, sequenceDiagram | Ch 43 Control flow |
| M-GAP-3 | GrowthBook 6 evaluation paths, 4 override layers, periodic refresh (6h/20min), remote eval caching, flowchart decision tree | Ch 50 Control flow |
| M-GAP-4 | **SKIPPED** — `src/services/teleport/` directory does not exist in source | N/A |
| M-GAP-5 | SDK interface: 27 hook events, 6 exit reasons, agent SDK functions (tool/query/createSession), control protocol (SDKControlRequest/Response), sequenceDiagram | Ch 44 Control flow |
| H-HARNESS-5 | Session handoff: `HandoffFile` Zod schema, `writeHandoffFile()` hooked into SessionEnd, `readHandoffFile()` at SessionStart, `buildHandoffSystemPrompt()` | Ch 55 |

### Phase 4: Harness Engineering Additions (4/4 done)

| ID | Content | Location |
|----|---------|----------|
| H-HARNESS-1 | Loop detection: `LoopDetector` class with sliding-window hash, `createLoopDetectorHook()` wired into `runPreToolUseHooks`, warn/block verdicts | Ch 56 |
| H-HARNESS-2 | Cost enforcement: `CostCap` with soft/hard/kill limits, `enforceCostCap()`, per-task attribution, flowchart | Ch 56 |
| H-HARNESS-3 | Distributed tracing: `TraceSpan` interface, 5-step trace propagation walkthrough, sequenceDiagram across 3 agent levels | Ch 55 |
| H-HARNESS-4 | Structural verification gate: `PreToolUse` hook on `TaskUpdate` checking transcript for verification evidence before allowing `completed` transition, sequenceDiagram | Ch 22 |

### Phase 5: Nice-to-Have (2/2 done)

| ID | Content | Location |
|----|---------|----------|
| H-HARNESS-6 | Minimal harness starter skeleton: 537-line TypeScript file with query loop, 3-tool registry, JSONL persistence, cost tracking | Appendix I |
| H-HARNESS-7 | Greenfield implementation ordering guide: 8 layers in optimal build order (1→2→4→3→5→6→7→8), Layer 4 before 3 rationale | Ch 56 |

### Final Verification Results

| Metric | Before (Session 1) | After (Session 2) | Delta |
|--------|--------------------|--------------------|-------|
| Lines | 29,388 | 32,164 | +2,776 |
| Words | 300,734 | 326,908 | +26,174 |
| Mermaid diagrams | 163 | 172 | +9 |
| TypeScript code blocks | 660 | 692 | +32 |
| Source citations | 2,458 | 2,674 | +216 |
| Forbidden tokens | 0 | 0 | 0 |
| Hash-comments in TS | 7 | 0 | fixed |
| Appendices | 3 (A-C) | 9 (A-I) | +6 |

## 4. Current State of Files

### Git Status
- Branch: `cc_research_book`
- Modified (unstaged): `deep-research-of-cc-source-code.md` (+2,896 insertions, -120 deletions)
- Untracked: `.book-build.run-{1,2,3}-aborted-*`, `.claude/`, `book-build-process-summary.md`, `book-review-report.md`, `tmp_prompt.md`
- Last commit: `6cd231b Add deep research book on CC source code (57 chapters, 300k words)`
- **The book changes from Session 2 are NOT committed yet.** The user should decide when/how to commit.

### Key Files (All Paths Absolute)

| File | Size | Purpose |
|------|------|---------|
| `/home/hwzhang/build/open-claude-code/deep-research-of-cc-source-code.md` | ~2.4 MB | The finished book (326,908 words, 32,164 lines, all fixes applied) |
| `/home/hwzhang/build/open-claude-code/book-review-report.md` | 19 KB | The review that drove Session 2 fixes (ALL items addressed except M-GAP-4) |
| `/home/hwzhang/build/open-claude-code/book-build-process-summary.md` | 37 KB | Process analysis from Session 1. Section 5 has the `/orchestrate` skill design. |
| `/home/hwzhang/build/open-claude-code/book-implementation-plan.md` | 162 KB | The master plan from Session 1 (reference implementation of harness pattern) |
| `/home/hwzhang/.vim/claude/harness/harness-engineering-report.md` | ~1300 lines | The Harness Engineering Report (HER) — theoretical framework |
| `/home/hwzhang/build/open-claude-code/.claude/handoff.md` | This file | Session handoff document |

### Book Structure After Fixes

```
57 chapters across 10 Parts
+ Appendix A. Glossary of Terms
+ Appendix B. Bibliography and Further Reading
+ Appendix C. Cross-Reference Concordance
+ Appendix D. Tool Reference Table (NEW — 50+ tools)
+ Appendix E. System Prompt Section Catalog (NEW)
+ Appendix F. Feature Flag Reference (NEW — 97 flags)
+ Appendix G. Hook Event Reference (NEW — 28 events)
+ Appendix H. Environment Variables (NEW — 200+ vars)
+ Appendix I. Minimal Harness Starter Skeleton (NEW — working TypeScript)
```

## 5. What the User Wants Next: Reusable Multi-Agent Harness Skill

The book and its fixes are DONE. The primary goal now is to **extract the orchestration patterns** used to build the book into a **reusable multi-agent harness skill/system**.

### Source Material for the Skill

The `book-build-process-summary.md` Section 5 contains a detailed skill design:

- **Separate mechanism from policy**: The dispatch loop, gates, convergence, STATUS parsing are identical across tasks (mechanism). The task definition, worker roles, quality criteria are task-specific (policy). The skill encodes mechanism; the user supplies policy.
- **Proposed skill name**: `/orchestrate`
- **Skill phases**: Brainstorm → Plan generation → Plan validation → Execution → Monitoring

### Architecture That Worked (from Session 1)

```
PARENT (thin dispatcher, <40% context)
├─ Step 0: bootstrap → manifest, repo-sha
├─ Step 0.5: template extraction → templates, excerpts, parent-state
├─ Step 0.9: verification gate → 12 checks, HALT on FAIL
├─ Step 1: work dispatch (N × batch-runners) → items drafted
├─ Step 2: prerequisites (ordered runner) → prerequisites completed
├─ Step 3: item audits (N × audit-runners, 6 auditors each) → verdicts
├─ Step 4: revisions (N × revision-runners) → fix flagged issues [DOMINANT: 47% of dispatches]
├─ Step 5: critical review (≤4 rounds × review-runners, 3 reviewers each)
├─ Step 6: cross-item audit (≤3 rounds × stitch-runners)
├─ Step 7: back matter (final-runner) → synthesis artifacts
└─ Step 8: final assembly (≤3 rounds × assembly-runners) → final output
```

### Core Design Principles (18 lessons)

1. **Thin dispatcher pattern**: Parent must NEVER read content — only structured status
2. **Templates on disk**: Instructions stored as files, subagents self-load from paths
3. **One-line STATUS contract**: Subagent returns = structured JSON, one line only
4. **Verification gates with structural enforcement**: File-existence checks, not prompt trust
5. **Manifest-driven state**: JSON state file is the source of truth, survives context compaction
6. **Convergence logging**: Append-only JSONL event log for audit trail and debugging
7. **Hard caps with escalation**: Every feedback loop needs an absolute limit and an escalation path
8. **Self-correction is the majority of work**: Plan for 50%+ of dispatches to be corrective
9. **Tiered re-verification**: After fixes, re-check only the flagged dimensions, not everything
10. **Explicit language enforcement**: "All output must be in English" in EVERY template
11. **Plan-as-firmware**: Validate the plan against ground truth before execution
12. **Permission bypass for automation**: Configure once with guardrails, then don't interrupt
13. **Monitoring agent pattern**: Separate context for watching, diagnosing, and patching
14. **Clean restarts over mid-session recovery**: Until the pipeline is stable
15. **Checkpoint-based resume**: Write checkpoint-N.json after each step for future restartability
16. **State machine transitions must be explicit**: "After this step, set status to X" in every template
17. **Separate per-item checks from cross-item checks**: Catch issues at item level, not assembly level
18. **Expect 2-4 iterations before stabilization**: Architecture → enforcement → configuration → success

### Known Behavioral Issues (GLM 5.1 specific)
- **Language drift**: Under heavy token load, switches to Chinese for status messages. Fix: "All output in English" in every dispatch prompt.
- **State machine gaps**: Revision runners did not advance manifest status. Fix: explicit status transitions in every template.
- **Forbidden token whack-a-mole**: Revision agents reintroduce "simply" while fixing other issues. Fix: per-item forbidden-token check before promoting to cross-item audit.

### Open Questions for the User (from Session 1, still unanswered)

1. Should the harness skill be a Claude Code plugin/skill (`.claude/skills/`) or a standalone framework?
2. Should the harness skill target GLM 5.1 specifically, or be model-agnostic?

## 6. Technical Setup

### fcoder (GLM 5.1 Launcher)

```bash
# Defined in ~/.zshrc as a function
function fcoder() {
  (
    export ANTHROPIC_AUTH_TOKEN="sk-1fd9d6aeb7795f00dae118b95b3c40bf2a2dfe495e8ac5c8b89941b7dc50aff1"
    export ANTHROPIC_BASE_URL="https://fos-exp-ai.corp.fortinet.com:443"
    export ANTHROPIC_DEFAULT_SONNET_MODEL="GLM 5.1"
    export ANTHROPIC_DEFAULT_OPUS_MODEL="GLM 5.1"
    export ANTHROPIC_DEFAULT_HAIKU_MODEL="GLM 5.1"
    export CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC=1
    export CLAUDE_AUTOCOMPACT_PCT_OVERRIDE=95
    export FTNT_CLAUDE_CONTEXT_WINDOW=202752
    export FCODER_MODE=1
    claude "$@"
  )
}
```

### Permission Configuration

The project `.claude/settings.json` uses `defaultMode: "bypassPermissions"` with allow/deny rules. User's global `~/.claude/settings.json` has `defaultMode: "plan"` which overrides project settings. Must launch with `--dangerously-skip-permissions` flag for unattended runs:
```bash
fcoder --dangerously-skip-permissions
```

### Verification Commands
```bash
FILE=deep-research-of-cc-source-code.md
echo "Lines:" && wc -l $FILE
echo "Words:" && wc -w $FILE
echo "Mermaid:" && grep -c '^```mermaid' $FILE
echo "TS blocks:" && grep -c '^```typescript' $FILE
echo "Citations:" && grep -oE 'src/[^ ]+\.tsx?(:L[0-9]+(-L[0-9]+)?)?' $FILE | wc -l
echo "Forbidden:" && grep -cE 'TODO|TBD|\[NEEDS-VERIFY\]|XXX' $FILE
echo "Hash-in-TS:" && python3 -c "
import re
text = open('$FILE').read()
for i, b in enumerate(re.findall(r'\`\`\`typescript\n(.*?)\`\`\`', text, re.DOTALL)):
    for line in b.split('\n'):
        if line.startswith('# src/'): print(f'Block {i}: {line}')
"
```

## 7. What NOT to Do

- Do NOT re-run the book build or the book fix process. Both are done.
- Do NOT commit without user approval. The user's global CLAUDE.md forbids AI attribution.
- Do NOT guess at the architecture — read `book-build-process-summary.md` for the verified patterns.
- Do NOT underestimate self-correction. It's 54% of the work, not a failure mode.
- Do NOT modify the book further unless the user asks. All review items are addressed.

## 8. Recommended Reading Order for New Session

1. **This file** (you're reading it now)
2. **`book-build-process-summary.md`** — Section 0 (self-correction insight), Section 1 (what works), Section 5 (skill recommendation)
3. **`book-review-report.md`** — For reference only; all items are now addressed
4. **The HER** at `/home/hwzhang/.vim/claude/harness/harness-engineering-report.md` — Sections 5 (12 patterns), 6 (17 failure modes), 20 (best practices), 21 (8-layer reference architecture)
5. **`book-implementation-plan.md`** Sections 5-8 — The actual dispatch architecture, templates, and rules. Only read if building the skill.
6. **`.book-build/templates/`** — The 27 actual templates used. Concrete examples of the subagent contract pattern.

## 9. Session 2 Execution Method (for reference)

Session 2 used 8 parallel subagents dispatched from a single Opus 4.6 session:
- Each subagent was given a specific chapter/section to modify
- Subagents read the source code, the other branch (`origin/source_code_deep_research`), and the existing book content before writing
- Subagents used the Edit tool (string matching, not line numbers) to insert content at precise locations
- Non-overlapping file regions allowed safe parallel execution
- The main session performed Phase 1 bug fixes directly, then dispatched subagents for Phases 2-5

This parallel-subagent-per-section approach could itself be encoded in the `/orchestrate` skill as a pattern.

---

**End of handoff. The new session should start by reading this file, then `book-build-process-summary.md` Section 5, then proceed with the harness skill extraction based on user direction.**
