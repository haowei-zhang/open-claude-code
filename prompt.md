# Prompt for GLM 5.1 — Write the CC Source Code Research Book

> **Operator note (read first, do not send):** This file is both a launcher and a prompt. The section below titled "PROMPT TO SEND" is what you paste into the GLM 5.1 session (via `fcoder`). Everything above it is setup guidance for the human operator. The sections below "PROMPT TO SEND" are recommended skills and operational notes that the model should invoke or follow once it starts.

---

## 0. Recommended launch procedure (operator)

### 0.1 Prerequisites
- The repo at `/home/hwzhang/build/open-claude-code` is the subject of the book and the write target. It should be on a stable commit; the plan pins `git rev-parse HEAD` at Step 0 so chapters are written against one SHA.
- The HER report must exist at `/home/hwzhang/.vim/claude/harness/harness-engineering-report.md`.
- The plan file must exist at `/home/hwzhang/build/open-claude-code/book-implementation-plan.md`.
- `fcoder` must be available in your shell (defined in `~/.zshrc`).

### 0.2 Permission pre-configuration
Because this task writes hundreds of files and runs many subagents, interactive permission prompts will block execution. Configure `~/.claude/settings.json` (or a repo-local `.claude/settings.json`) before launching:

```json
{
  "permissions": {
    "allow": [
      "Read(/home/hwzhang/build/open-claude-code/**)",
      "Read(/home/hwzhang/.vim/claude/harness/harness-engineering-report.md)",
      "Read(/home/hwzhang/build/open-claude-code/book-implementation-plan.md)",
      "Read(/home/hwzhang/build/open-claude-code/.book-build/**)",
      "Write(/home/hwzhang/build/open-claude-code/.book-build/**)",
      "Write(/home/hwzhang/build/open-claude-code/deep-research-of-cc-source-code.md)",
      "Edit(/home/hwzhang/build/open-claude-code/.book-build/**)",
      "Edit(/home/hwzhang/build/open-claude-code/deep-research-of-cc-source-code.md)",
      "Bash(git -C /home/hwzhang/build/open-claude-code rev-parse *)",
      "Bash(git -C /home/hwzhang/build/open-claude-code show *)",
      "Bash(wc *)",
      "Bash(grep *)",
      "Bash(ls *)",
      "Bash(mkdir -p /home/hwzhang/build/open-claude-code/.book-build/**)"
    ],
    "deny": [
      "Write(/home/hwzhang/build/open-claude-code/src/**)",
      "Edit(/home/hwzhang/build/open-claude-code/src/**)",
      "Bash(rm -rf *)",
      "Bash(git push *)",
      "Bash(git commit *)"
    ]
  }
}
```

The `deny` list is important: it prevents accidental modification of the source tree and any git mutations. GLM 5.1 should never touch cc's source code while writing the book.

Alternative: launch with `--permission-mode acceptEdits` to auto-approve edits (less safe, faster). Do **not** use `bypassPermissions`; the deny list above is your safety net.

### 0.3 Launching fcoder

**Recommended — interactive session in a dedicated terminal:**
```bash
cd /home/hwzhang/build/open-claude-code
fcoder
```
Then paste the "PROMPT TO SEND" block below into the session.

**Alternative — one-shot headless run (for fire-and-forget):**
```bash
cd /home/hwzhang/build/open-claude-code
fcoder -p "$(sed -n '/^## PROMPT TO SEND/,/^## END PROMPT$/p' prompt.md | sed '1d;$d')"
```
Headless mode skips the REPL and runs to completion in the background. Use this only if you trust the permissions config. Redirect output: `… 2>&1 | tee fcoder-run.log`.

**Why an interactive terminal is preferred for this particular task:**
- The build will run for many hours (likely 8–30 hours of wall clock).
- You want visibility into audit rounds and early failure signals.
- You can `Ctrl+C` and resume (the plan's Section 13 "Resume Protocol" makes every step idempotent).
- Terminal session > background process for a multi-hour task because you can inspect state and intervene.

---

## 1. What skills GLM 5.1 will use (FYI only — you do nothing)

> **You do not need to act on this section.** It's a preview so you know what GLM 5.1 will invoke automatically when you paste the prompt. GLM reads the prompt, sees "invoke skill X", and calls its own `Skill` tool.

- `superpowers:using-superpowers` — Turn 1, establishes skill discipline
- `superpowers:executing-plans` — Turn 1, loads the plan-execution pattern

The hierarchical model (parent → step-runner → worker) means the parent doesn't need to invoke `subagent-driven-development` or `dispatching-parallel-agents` itself — those are concerns of the step-runners, which may invoke them inside their own contexts. The parent is a pure dispatcher that only needs the two skills above.

---

## 2. Operational expectations

- **Parent context should stay under 40% throughout.** If it climbs above 40%, a rail was broken. The hierarchical architecture keeps the parent thin: initial plan read adds ~10% overhead, then ~500 tokens per step-runner dispatch across ~100 dispatches adds ~25%, totaling ~35% of 202k. Above 40% means something leaked context (e.g., a subagent returned prose instead of STATUS).
- **Cost is significant.** Expect ~100 step-runner dispatches at the parent level + several hundred worker subagents dispatched by step-runners. Each chapter flows through 1 writer + 6 auditors + possibly revisers + critical review ≈ 8-15 subagent runs per chapter × 57 chapters ≈ 500–800 worker subagent dispatches total.
- **Time is significant.** Expect 10–30 hours wall clock.
- **The run is resume-safe.** If the fcoder session crashes, re-launching with the same prompt will pick up from `manifest.json` state (plan Section 13).
- **Step 0.9 is a hard gate.** If verification fails, the run halts. This is intentional — better to fail fast than drift silently.
- **You prefer restart over recovery.** Every restart is 10+ hours wasted, so the plan is built to prevent deviation upfront rather than recover from it. Hard rails everywhere.

---

## PROMPT TO SEND

*Everything below this line and above "## END PROMPT" is what you paste into the fcoder session.*

---

You are GLM 5.1 running inside Claude Code. Your role is the **THIN ORCHESTRATOR PARENT** of a hierarchical book build. You do not write chapters. You do not validate files. You do not extract templates. You do not read source code. You do not read chapter bodies. You do not read HER. You do not read the plan file after this turn.

**Your entire job is**: dispatch step-runner subagents, read their one-line STATUS responses, verify via `manifest.json` and `parent-state.md`, and move to the next step. That is all.

### Inputs

1. **The plan**: `/home/hwzhang/build/open-claude-code/book-implementation-plan.md` — 2,395 lines. You read Sections 1–7 ONCE this turn (~1,170 lines via a single Read with offset=1, limit=1170). You never re-read any part of the plan after that. Section 8 starts at ~line 1169 — do NOT read it. Section 8 templates are extracted to `.book-build/templates/*.md` by the step-0.5-runner and read only by step-runners and workers — not by you.

2. **Canonical source corrections**: Section 6.5 of the plan lists the files that are missing from the leaked repo and their replacements. The step-0-runner applies these automatically. You do NOT re-discover missing files, you do NOT re-validate paths, you do NOT spend any time on this — it is a solved problem baked into the plan.

### Outputs

1. **Final book**: `/home/hwzhang/build/open-claude-code/deep-research-of-cc-source-code.md` (written once by the step-8-final-runner)
2. **Build artifacts**: `/home/hwzhang/build/open-claude-code/.book-build/`
3. **Escape hatch**: `.book-build/convergence-failures.md` (only if convergence fails)

### Hard rails (violating any of these breaks the run)

1. **The parent is a pure dispatcher.** You never call Write, Edit, or any tool that produces files. You never call Bash for work that isn't a read-only check on the allowed paths.
2. **Forbidden reads (HARD list — do NOT call Read on any of these)**:
   - Any file under `/home/hwzhang/build/open-claude-code/src/**`
   - Any file under `.book-build/chapters/**`
   - Any file under `.book-build/her-excerpts/**`
   - Any file under `.book-build/templates/**`
   - The HER report at `~/.vim/claude/harness/harness-engineering-report.md`
   - Any audit prose report (`.book-build/audits/**/*.md` — only read the sibling `.verdict.json` files)
   - The plan file after this turn's initial read
3. **Allowed reads (the COMPLETE list — read nothing else)**:
   - `book-implementation-plan.md` Sections 1–7 (this turn only, one Read call with offset+limit)
   - `.book-build/parent-state.md`
   - `.book-build/manifest.json`
   - `.book-build/repo-sha.txt`
   - `.book-build/loc-hints/files.json`
   - `.book-build/convergence.log` (tail only)
   - `.book-build/audits/chNN/*.verdict.json` (tiny JSON only)
   - `.book-build/stitch-verdict.json`
   - `.book-build/critical-reviews/pass-NN-merged.json`
   - `.book-build/verification-0.9.md` (after Step 0.9)
4. **You never dispatch a writer, auditor, reviser, or stitch-auditor directly.** You dispatch only step-runners. Step-runners dispatch workers. Three-level hierarchy.
5. **Every subagent you dispatch returns EXACTLY ONE LINE** — the `STATUS: {...}` JSON. You read only that line. Any prose a subagent emits is a plan violation and should trigger a failure.
6. **After every step-runner DONE, you re-read `parent-state.md` and `manifest.json`.** That is how you stay in sync.
7. **You never skip a step. You never skip a verification gate.** If Step 0.9 returns FAIL, you HALT and report to the operator.
8. **You never paraphrase the chapter table.** The step-0-runner copies §6.3 verbatim.
9. **Your context usage should stay below 40% throughout the run.** The initial plan read adds ~10%, plus ~500 tokens per dispatch. If it rises above 40%, something is wrong — a subagent returned prose instead of STATUS, or you read something forbidden. Compact with the exact prompt in plan §5.5.6 and investigate.

### Your execution sequence (follow this EXACTLY)

**Turn 1 (this turn — setup and confirmation):**

1. Invoke skill `superpowers:using-superpowers`
2. Invoke skill `superpowers:executing-plans`
3. Read `/home/hwzhang/build/open-claude-code/book-implementation-plan.md` with `offset=1, limit=1170`. This reads Sections 1–7 only (§8 starts at ~line 1169). Do NOT read Section 8+. Do NOT re-read the plan later.
4. Verify `/home/hwzhang/build/open-claude-code/src/` exists (one `ls` is fine)
5. Verify `/home/hwzhang/.vim/claude/harness/harness-engineering-report.md` exists (one `ls` is fine — do NOT read it)
6. Emit this exact confirmation (3 sentences, no more): "I am the thin orchestrator parent. I have read plan Sections 1–7. I will now dispatch step-0-runner, then step-0.5-runner, then step-0.9-runner verification gate, then Step 1 batch runners in sequence — never executing step logic myself."
7. Proceed to Turn 2 to dispatch the step-0-runner.

**Turn 2: Step 0 dispatch.**

Dispatch ONE step-0-runner subagent via the Agent tool with this prompt:
```
You are the step-0-runner. Read /home/hwzhang/build/open-claude-code/book-implementation-plan.md Section 8.16.1 (use offset/limit to target only that section) for your full instructions. On this first run the template file .book-build/templates/step-0-runner.md does not yet exist — read §8.16.1 of the plan directly. Apply the canonical source file corrections from plan §6.5 WITHOUT re-validation. Execute Step 0 end-to-end. Your final response is exactly one line: STATUS: {"status":"done"|"failed","step":"0","chapters":57,"missing_files":<int>,"renamed_files":<int>,"loc_hints":<int>}.
```

Wait for STATUS. Read ONLY the STATUS line. If `status="failed"`, HALT and report. If `status="done"`:
- Read `.book-build/manifest.json` (only to verify 57 chapters exist — `jq '.chapters | length'` is acceptable)
- Read `.book-build/repo-sha.txt`
- Proceed to Turn 3.

**Turn 3: Step 0.5 dispatch.**

Dispatch ONE step-0.5-runner subagent with this prompt:
```
You are the step-0.5-runner. Read /home/hwzhang/build/open-claude-code/book-implementation-plan.md Section 8.16.2 (use offset/limit) for your full instructions. Execute Step 0.5 end-to-end: extract 27 templates (16 worker + 11 step-runner) from plan Sections 8.1–8.16 to .book-build/templates/*.md, extract 57 HER excerpts to .book-build/her-excerpts/chNN.md, write .book-build/parent-state.md (~1000 words, 8 sections per plan §5.5.5). Your final response is exactly one line: STATUS: {"status":"done"|"failed","step":"0.5","worker_templates":<int>,"step_runner_templates":<int>,"her_excerpts":<int>,"parent_state_bytes":<int>}.
```

Wait for STATUS. If `status="failed"`, HALT. If `status="done"`, verify counts in STATUS (expect 16 worker, 11 step-runner, 57 HER excerpts). Proceed to Turn 4.

**Turn 4: Step 0.9 verification gate.**

Dispatch ONE step-0.9-runner subagent with this prompt:
```
You are the step-0.9-runner. Read .book-build/templates/step-0.9-runner.md for your full instructions. Run every verification check. Your final response is exactly one line: STATUS: {"status":"pass"|"fail","step":"0.9","checks_passed":<int>,"checks_failed":<int>,"failed_checks":[<names>]}.
```

Wait for STATUS. If `status="fail"`:
- Read `.book-build/verification-0.9.md` for the detailed failure report
- HALT and report the failed checks to the operator
- Do NOT proceed to Step 1

If `status="pass"`, proceed to Turn 5.

**Turns 5+: Step 1 batch dispatch (14 or 15 batches).**

For each batch in order (batch 1 = Ch 1–4, batch 2 = Ch 5–8, …, batch 14 = Ch 53–56, batch 15 = Ch 57):
- Dispatch a step-1-batch-runner with this prompt:
  ```
  You are the step-1-batch-runner for batch {{N}} (chapters {{ch_range}}). Read .book-build/templates/step-1-batch-runner.md for instructions. Read .book-build/parent-state.md and .book-build/manifest.json. Execute the batch end-to-end. Your final response is exactly one line: STATUS: {"status":"done"|"failed","step":"1","batch":{{N}},"chapters_drafted":<int>,"chapters_failed":[<ids>]}.
  ```
- At most 2 step-1-batch-runners in flight at any time.
- **Part I first**: batch 1 must complete before any non-Part-I batches dispatch.
- After each BATCH_DONE, read `manifest.json` to confirm chapters are at `status="drafted"`. Re-read `parent-state.md` if context feels high.
- If any chapter is in `chapters_failed`, dispatch another step-1-batch-runner for just that chapter on the next free slot.

**Subsequent turns: Steps 2–8 similarly, each dispatched as ONE step-runner per unit.**

- Step 2 (only after Part I + Part II batches DONE): dispatch ONE step-2-runner, await DONE
- Step 3: dispatch step-3-chapter-runners, 2 in flight, one per chapter (57 total dispatches)
- Step 4: for each chapter that needs revision, dispatch step-4-revision-runner → re-enter Step 3 for that chapter
- Step 5: dispatch step-5-critical-runner per round (up to 4 rounds)
- Step 6: dispatch step-6-stitch-runner per round (up to 3 rounds)
- Step 7: dispatch ONE step-7-backmatter-runner
- Step 8: dispatch ONE step-8-final-runner. If `status="needs_revisions"`, dispatch step-4-revision-runners for affected chapters, re-dispatch step-8-final-runner. Up to 3 Step 8 rounds.

**Declare DONE when** two consecutive Step 8 runs produce zero revisions AND the book is done per plan §10.2.

### When you are done

Emit a final status message with: total chapters completed / needs_human_review / failed; total step-runner dispatches; total wall-clock time; final word count / mermaid / snippet / citation counts; path to final file; path to convergence.log; any outstanding issues.

### What to do if something goes wrong

- **A step-runner returns `status="failed"`**: HALT. Do not retry without operator input. The failure is in the step-runner's STATUS and (usually) in convergence.log.
- **A step-0.9 check fails**: HALT. The build state is broken. Operator decides whether to wipe `.book-build/` and restart or fix and retry.
- **Your context climbs above 40%**: something is wrong. Compact with plan §5.5.6 prompt, then investigate what bloated you (usually a subagent returning prose instead of STATUS).
- **A subagent returns prose instead of a single STATUS line**: treat as failure. The rail is broken.

**Begin now. Turn 1, step 1: invoke `superpowers:using-superpowers`.**

## END PROMPT

---

## 3. Post-launch monitoring (operator)

While GLM 5.1 runs, you can watch progress from another terminal:

```bash
# Tail convergence log
tail -f /home/hwzhang/build/open-claude-code/.book-build/convergence.log

# Count drafted vs audited chapters
jq '.chapters | group_by(.status) | map({status: .[0].status, count: length})' \
  /home/hwzhang/build/open-claude-code/.book-build/manifest.json

# Watch directory growth
watch -n 10 'ls /home/hwzhang/build/open-claude-code/.book-build/chapters/ | wc -l; du -sh /home/hwzhang/build/open-claude-code/.book-build/'
```

If GLM 5.1 appears stuck (no `convergence.log` updates for > 10 minutes), `Ctrl+C` the session. Re-launching with the same prompt resumes from `manifest.json` state (idempotent by design, per plan Section 13).

## 4. Verification after completion

Run the 11 checks from Section 12 of the plan:

```bash
FILE=/home/hwzhang/build/open-claude-code/deep-research-of-cc-source-code.md
ls -l $FILE                                                    # exists
wc -w $FILE                                                    # 200000-310000
grep -c '^```mermaid' $FILE                                    # ≥ 114
grep -cE '^(//|#) src/' $FILE                                  # ≥ 228 snippets
grep -oE 'src/[^ ]+\.tsx?(:L[0-9]+(-L[0-9]+)?)?' $FILE | wc -l  # ≥ 342
grep -cE 'TODO|TBD|\[NEEDS-VERIFY\]' $FILE                     # 0
```

Plus sampled checks: spot-check 5 random chapters for the 6 mandatory sections, 20 random citations against the pinned SHA, 10 random code snippets against the source, 10 random chapters for the divergence section.
