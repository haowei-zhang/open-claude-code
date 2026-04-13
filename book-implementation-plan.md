# Deep Research and Development Guide of CC Source Code — Implementation Plan

> **For GLM 5.1 (executing agent):** This plan is self-contained. Read it end-to-end **before** dispatching any subagent. Every file path, step, subagent contract, and convergence rule you need is in this document. Do not paraphrase the chapter table — copy it verbatim into `manifest.json`.

---

## 0. Context

### 0.1 Why this book is being written
The user is building a next-generation coding agent with a sub-agent dispatcher and harness, intended to run **long, complicated, autonomous tasks reliably** (hours or days of wall-clock time). They have:

1. A leaked reference implementation of Claude Code at `/home/hwzhang/build/open-claude-code` (`cc` for short) — the most mature public example of a long-running agent harness.
2. A ~1300-line harness engineering research report at `~/.vim/claude/harness/harness-engineering-report.md` (hereafter **HER**) synthesizing 60+ academic, industry, and practitioner sources on harness engineering as a discipline.

The book being planned will become the bridge between those two artifacts: it must deeply, accurately, comprehensively document every significant subsystem of cc **and** cross-reference HER so that any future engineer (human or AI) can use the book as both:

- a **forensic reference** for what cc actually does, file by file, and
- a **design guide** for building a trustworthy long-running agent.

### 0.2 Repository snapshot (verified in Phase 1 exploration)
- ~27,500 lines of TypeScript/TSX across ~1,884 files
- Runtime: Bun (primary) + Node 18+ compatibility
- UI: custom React-like Ink terminal renderer
- Major subsystems: `src/entrypoints/`, `src/query*`, `src/services/`, `src/tools/` (44+), `src/utils/permissions/`, `src/hooks/`, `src/skills/`, `src/memdir/`, `src/tasks/`, `src/coordinator/`, `src/bridge/`, `src/ink/`, `src/state/`, `src/bootstrap/`, `src/assistant/` (KAIROS), `src/buddy/`, `src/vim/`, `src/upstreamproxy/`
- No automated test suite detected
- `README.md` indicates the code was sourced via a March 2026 npm sourcemap leak

### 0.3 Shared ideas between cc and HER (key cross-references)
The HER report identifies 12 "Claude Code-derived patterns", 17 failure modes, 6 configuration surfaces, 5-layer defense-in-depth, and an 8-layer reference architecture. Every one of these maps to concrete code in cc. **The book's primary value is turning those abstract patterns back into verifiable code citations.** Specific mappings:

| HER concept | cc implementation (representative files) |
|---|---|
| Pattern 1 Persistent Instruction File | `src/utils/claudemd.ts`, `src/constants/prompts.ts` |
| Pattern 2 Scoped Context Assembly | `src/utils/settings/settings.ts`, `src/utils/analyzeContext.ts` |
| Pattern 3 Tiered Memory | `src/memdir/*`, `src/services/SessionMemory/*` |
| Pattern 4 Dream Consolidation | `src/services/autoDream/*`, `src/tasks/DreamTask/*` |
| Pattern 5 Progressive Context Compaction | `src/services/compact/*`, `src/utils/messages.ts` |
| Pattern 6 Explore-Plan-Act Loop | `src/tools/EnterPlanModeTool/*`, `src/utils/planModeV2.ts`, `src/query.ts` |
| Pattern 7 Context-Isolated Subagents | `src/tools/AgentTool/*`, `src/utils/forkedAgent.ts` |
| Pattern 8 Fork-Join Parallelism | `src/tools/EnterWorktreeTool/*`, `src/utils/worktree.ts`, `src/coordinator/*` |
| Pattern 9 Progressive Tool Expansion | `src/tools/ToolSearchTool/*`, `shouldDefer` flag in `src/Tool.ts` |
| Pattern 10 Command Risk Classification | `src/utils/permissions/bashClassifier.ts`, `yoloClassifier.ts` |
| Pattern 11 Single-Purpose Tool Design | `src/Tool.ts`, `src/tools/*` taxonomy |
| Pattern 12 Deterministic Lifecycle Hooks | `src/schemas/hooks.ts`, `src/utils/hooks.ts`, `src/utils/hooks/*` |
| Failure mode 6.1 Context Rot | `src/services/compact/*` |
| Failure mode 6.2 Premature Completion | `src/query/stopHooks.ts` |
| Failure mode 6.4 Placeholder Implementations | `src/constants/prompts.ts` system prompt |
| Failure mode 6.6 Silent Failures | `src/services/tools/toolHooks.ts` |
| Failure mode 6.11 Cost Explosion | `src/utils/cost-tracker.ts`, `src/services/analytics/*` |
| Failure mode 6.13 Prompt Injection | `src/utils/hooks/ssrfGuard.ts`, `src/services/mcp/channelAllowlist.ts` |
| 8-layer reference architecture | Maps to `src/tasks/`, `src/state/`, `src/services/compact/`, `src/services/tools/`, `src/utils/permissions/`, `src/services/analytics/`, `src/coordinator/`, `src/services/autoDream/` |

The book must restate and elaborate every such mapping with file:line citations.

---

## 1. Objective and Definition of Done

### 1.1 Objective
Produce a single Markdown file at `/home/hwzhang/build/open-claude-code/deep-research-of-cc-source-code.md` containing:

- **Title**: "Deep Research and Development Guide of CC Source Code"
- **57 chapters** organized into **10 parts** plus **3 appendices**, plus front matter and part intros
- **≥ 200,000 and ≤ 310,000 words** total
- **≥ 114 mermaid diagrams** (mean ≥ 2 per chapter; many chapters will have 3)
- **≥ 342 verifiable source-file citations** in the form `src/path/to/file.ts:L123` (mean ≥ 6 per chapter)
- **≥ 228 quoted code snippets** from the cc codebase (mean ≥ 4 per chapter) — fenced TypeScript/TSX code blocks containing **verbatim** excerpts from the actual files, each prefixed by a citation comment identifying the source path and line range. See Section 6.1 and Writer template in §8.1 for the exact format.
- **Every HER pattern, failure mode, configuration surface, and best practice** cross-referenced at least once, with file evidence

### 1.2 Success criteria ("100% perfect" operationally defined)
A deterministic convergence rule is used in place of the non-computable "100% perfect":

**The book is DONE iff two consecutive Step 8 rounds produce zero revision dispatches AND every rule in Section 10 "Quality Criteria and Convergence" evaluates `pass` for every chapter AND the final stitched file passes the global sanity pass in Section 7 Step 8.**

### 1.3 Escape hatch
If the book has not converged after **10** full audit rounds, GLM 5.1 writes `.book-build/convergence-failures.md` listing every remaining issue and halts gracefully rather than spinning forever. It emits a final status message to the user summarizing what is unresolved and which chapters are still blocking.

---

## 2. Input Artifacts (what GLM 5.1 reads, never writes)

| Artifact | Path | Purpose |
|---|---|---|
| cc source tree | `/home/hwzhang/build/open-claude-code/` | Primary subject of the book. Readable only. |
| HER report | `/home/hwzhang/.vim/claude/harness/harness-engineering-report.md` | Cross-reference target. Readable only. |
| This plan | `/home/hwzhang/build/open-claude-code/book-implementation-plan.md` | The source of truth for chapter structure, subagent contracts, quality rules, and workflow. |

**Constraint:** Subagents must NEVER write to or modify any path inside `/home/hwzhang/build/open-claude-code/` except the two paths listed in Section 3. The parent must verify this before stitching.

---

## 3. Output Artifacts (what GLM 5.1 writes)

| Artifact | Path | Role |
|---|---|---|
| Final book | `/home/hwzhang/build/open-claude-code/deep-research-of-cc-source-code.md` | The single shippable artifact. Written only at Step 8. |
| Build directory | `/home/hwzhang/build/open-claude-code/.book-build/` | All intermediate files, manifests, audits, excerpts. Preserved after success for re-iteration. |
| Convergence failure report | `/home/hwzhang/build/open-claude-code/.book-build/convergence-failures.md` | Only if escape hatch triggers. |

Both output paths are explicitly whitelisted. No other writes are permitted.

---

## 4. Build Directory Layout (`.book-build/`)

```
.book-build/
├── manifest.json                  # chapter registry (see Section 9)
├── terminology.json               # terms used consistently across chapters
├── convergence.log                # append-only log of every pass + verdict
├── repo-sha.txt                   # git rev-parse HEAD pinned at Step 0
├── frontmatter.md                 # title/TOC/preface/how-to-read
├── parts/
│   ├── part-01-intro.md           # ~500-900 words per part intro
│   ├── part-02-intro.md
│   └── ... (10 total)
├── chapters/
│   ├── ch01-why-this-book-exists.md
│   ├── ch02-book-architecture.md
│   └── ... (57 total)
├── her-excerpts/                  # pre-extracted HER slices per chapter
│   ├── ch01.md
│   └── ... (57 total)
├── loc-hints/                     # pre-computed wc-l for every source file in manifest
│   └── files.json
├── audits/
│   ├── ch01/
│   │   ├── accuracy.md
│   │   ├── accuracy.verdict.json
│   │   ├── cross-ref.md
│   │   ├── cross-ref.verdict.json
│   │   ├── diagrams.md
│   │   ├── diagrams.verdict.json
│   │   ├── polish.md
│   │   ├── polish.verdict.json
│   │   ├── consistency.md
│   │   ├── consistency.verdict.json
│   │   ├── gaps.md
│   │   └── gaps.verdict.json
│   └── ... (57 total)
├── critical-reviews/
│   ├── pass-01.md
│   └── ...
├── stitch-verdict.json            # from global stitch audit
└── back/
    ├── glossary.md
    ├── bibliography.md
    └── concordance.md
```

**Mandatory invariants:**
- One subagent writes to exactly one file. No shared writes.
- Parent never modifies chapter files; it only dispatches writers/revisers.
- Parent reads only: `manifest.json`, `*.verdict.json`, `terminology.json`, `convergence.log`, and during Step 8 the stitched output for the final sanity pass.

---

## 5. Execution Model and Roles

### 5.1 Parent session (GLM 5.1 main thread)
Acts as a **pure orchestrator** and **thin dispatcher**. It never drafts prose. It never writes files (step-runners handle all writes including manifest updates). Its jobs:
- Dispatch step-runner subagents (never workers directly)
- Read structured JSON verdicts and manifest state (read-only)
- Verify `manifest.json` state after each step-runner DONE
- Decide which step-runner to dispatch next based on verdicts and pass counts
- Maintain context hygiene (see Section 5.5)

### 5.2 Subagent roles (all run with fresh context, one task each)

| Role | Count per book | Purpose |
|---|---|---|
| **Writer** | 57 initially + re-dispatches | Writes a single chapter body to `chapters/chNN-slug.md` |
| **Part Intro Writer** | 10 | Writes one part intro to `parts/part-NN-intro.md` |
| **Frontmatter Writer** | 1 | Writes `frontmatter.md` (title page, TOC, preface, how-to-read) |
| **Accuracy Auditor** | ≥ 57 (one per chapter, possibly re-run) | Verifies every source citation against the actual file |
| **Cross-ref Auditor** | ≥ 57 | Verifies every HER citation against `her-excerpts/chNN.md` and the full HER |
| **Diagrams Auditor** | ≥ 57 | Parses every mermaid block, flags invalid syntax or useless diagrams |
| **Polish Auditor** | ≥ 57 | Prose quality, forbidden-word grep, clarity, voice |
| **Consistency Auditor** | ≥ 57 | Terminology drift, voice drift, reference to `terminology.json` |
| **Gaps Auditor** | ≥ 57 | Asks "what did the chapter fail to cover that the brief requires?" |
| **Reviser** | variable | Surgically fixes one or two flagged issues in an existing chapter |
| **Critical Reviewer** | 3 per round × ≤ 4 rounds = ≤ 12 | Samples chapters, challenges depth, identifies weak chapters |
| **Stitch Auditor** | 1 per round × ≤ 3 rounds = ≤ 3 | Cross-chapter continuity, numbering, TOC alignment, forward-reference resolution |
| **Back-matter Writer** | 3 | Glossary, bibliography, concordance |
| **Back-matter Auditor** | 3 | Accuracy check for back matter |

### 5.3 Parallelism caps (hard limits)
- **Writers**: max 4 concurrent per batch, max 2 batches in flight (effective: 8 concurrent). Start with Part I chapters first (they fix terminology), then batch subsequent parts 4 at a time.
- **Auditors**: max 12 concurrent globally (2 step-3-chapter-runners × 6 audit types in flight).
- **Critical reviewers**: 3 concurrent per round (inside a single step-5-critical-runner).
- **Revisers**: up to 4 concurrent, share the writer cap.
- **Stitch and backmatter**: sequential; only 1 each at a time.

### 5.4 Context hygiene (non-negotiable rules)
1. **Parent never embeds chapter bodies in prompts.** It embeds only file paths and verdict JSON.
2. **Every subagent is fresh.** No subagent resumes a previous subagent's context. Every dispatch is a new agent with a full brief.
3. **Parent reads only:** `manifest.json`, `*.verdict.json`, `terminology.json`, `convergence.log`, `stitch-verdict.json`, `repo-sha.txt`, `loc-hints/files.json`, `parent-state.md`, and during the Step 8 final sanity pass the assembled output file.
4. **Subagents that need another chapter's text read it directly from `chapters/chNN.md`.** Parent does not mediate.
5. **Every prompt template comes from a file in `.book-build/templates/` (see Section 5.5 and Step 0.5).** The parent does NOT re-embed the plan's Section 8 contents inline — it reads the template file or passes its path to the subagent. Deviation is a bug.
6. **No subagent is ever asked to read more than ~25 source files.** If a chapter needs more coverage, split it (handled at plan-writing time, not runtime).

### 5.5 Parent context discipline (HARD RAILS — deviation breaks the run)

The parent orchestrator session must survive a multi-hour run across ~100 step-runner dispatches. The parent is a **pure thin dispatcher**. It never does step work directly. It never reads source code, chapter bodies, HER content, or subagent work products. Its context stays under 15% throughout and compaction should never be needed.

**The budget**: ~200k effective context tokens. If the parent is a pure dispatcher per the rails below, it accumulates ~500 tokens per step-runner dispatch (prompt + one-line STATUS response). Across ~100 step-runner dispatches total across all steps, that's ~50k tokens = 25% max. Everything above 25% is a symptom that a rail was broken.

#### 5.5.1 Parent forbidden reads (hard list — parent MUST NOT call Read on these)

The parent is **forbidden** from calling the Read tool on any of these paths:

1. **Any source file** under `/home/hwzhang/build/open-claude-code/src/**` — source files are read only by worker subagents inside step-runners.
2. **Any chapter body** under `.book-build/chapters/**` — chapter bodies are read only by auditors, revisers, and the stitch subagent.
3. **Any HER excerpt** under `.book-build/her-excerpts/**` — HER excerpts are read only by writer subagents.
4. **The full HER report** at `/home/hwzhang/.vim/claude/harness/harness-engineering-report.md` — only the step-0.5-runner reads this.
5. **Any template body** under `.book-build/templates/**` — templates are read by step-runners when they dispatch workers, never by the parent.
6. **Plan file Sections 8.1 through 8.15** — templates live on disk after Step 0.5. Parent does not re-read the plan for templates. Parent does not re-read the plan at all after the initial Sections 1–7 read at session start.
7. **Any audit prose report** (`.book-build/audits/**/*.md`) — parent reads only the `.verdict.json` siblings.
8. **Any subagent's tool-call output or intermediate messages** — the Agent tool returns only the subagent's final message, which by Section 8.0 rule 1 is exactly one STATUS line.
9. **Any file under `/tmp/**` created during step-runner work** — step-runners handle their own temp files.
10. **Directory listings of large trees** (`ls /home/hwzhang/build/open-claude-code/src/` or similar with > ~50 entries) — step-runners handle exploration.

#### 5.5.2 Parent allowed reads (the complete list — parent must not read anything else)

The parent is allowed to call Read on these paths, and **only** these paths:

| Path | Purpose | Frequency |
|---|---|---|
| `book-implementation-plan.md` (Sections 1–7 + 6.3 chapter table + 6.5 source overrides) | Initial rule ingest at session start | Once per session |
| `.book-build/parent-state.md` | Rehydrate rules | After every step-runner DONE |
| `.book-build/manifest.json` | Track chapter state | After every step-runner DONE |
| `.book-build/repo-sha.txt` | Confirm SHA pinning | Once at startup |
| `.book-build/convergence.log` (tail only, last ~30 lines) | Verify events logged | After every step-runner DONE |
| `.book-build/audits/chNN/*.verdict.json` | Read audit outcomes (tiny JSON) | After each audit batch |
| `.book-build/stitch-verdict.json` | Read stitch outcome | Once per stitch round |
| `.book-build/critical-reviews/pass-NN-merged.json` | Read merged critical review outcomes | Once per critical round |
| `.book-build/loc-hints/files.json` | Confirm Step 0 completed | Once after Step 0 |

If the parent needs information not in this list, the answer is: **dispatch a step-runner subagent to get it, do not read anything else directly**.

#### 5.5.3 Parent forbidden writes

The parent is forbidden from writing any file. It is a pure dispatcher. All writes go through step-runners or their worker subagents. The parent's only file operations are Read (bounded by 5.5.2) and Bash for dispatching and running read-only checks like `wc` or `grep` on the allowed paths.

**Exception**: the parent MAY append a single event line to `.book-build/convergence.log` after confirming a step-runner's DONE response — but this is optional; the step-runner should do its own logging. If in doubt, do not write.

#### 5.5.4 Template files and step-runner contract

At Step 0.5 a step-0.5-runner subagent extracts Sections 8.1 through 8.15 of this plan to individual markdown files in `.book-build/templates/` — one file per template. Filenames:

- `templates/chapter-writer.md` (from §8.1)
- `templates/frontmatter-writer.md` (§8.2)
- `templates/part-intro-writer.md` (§8.3)
- `templates/accuracy-auditor.md` (§8.4)
- `templates/crossref-auditor.md` (§8.5)
- `templates/diagrams-auditor.md` (§8.6)
- `templates/polish-auditor.md` (§8.7)
- `templates/consistency-auditor.md` (§8.8)
- `templates/gaps-auditor.md` (§8.9)
- `templates/reviser.md` (§8.10)
- `templates/critical-reviewer.md` (§8.11)
- `templates/stitch-auditor.md` (§8.12)
- `templates/glossary-writer.md` (§8.13)
- `templates/bibliography-writer.md` (§8.14)
- `templates/concordance-writer.md` (§8.15)
- `templates/back-matter-auditor.md` (§8.15.1)

Step-runners (see Section 8.16) read the appropriate template and dispatch worker subagents. Step-runners themselves also have their own templates (Sections 8.16.1 through 8.16.11) and are written to `.book-build/templates/` as:

- `templates/step-0-runner.md` (§8.16.1)
- `templates/step-0.5-runner.md` (§8.16.2)
- `templates/step-0.9-runner.md` (§8.16.3)
- `templates/step-1-batch-runner.md` (§8.16.4)
- `templates/step-2-runner.md` (§8.16.5)
- `templates/step-3-chapter-runner.md` (§8.16.6)
- `templates/step-4-revision-runner.md` (§8.16.7)
- `templates/step-5-critical-runner.md` (§8.16.8)
- `templates/step-6-stitch-runner.md` (§8.16.9)
- `templates/step-7-backmatter-runner.md` (§8.16.10)
- `templates/step-8-final-runner.md` (§8.16.11)

Parent never reads any template file itself. When dispatching a step-runner, parent passes the template path in the dispatch prompt; the step-runner reads its own template.

#### 5.5.5 Parent state snapshot (`parent-state.md`)

At Step 0.5 the step-0.5-runner writes `.book-build/parent-state.md` — a ~1,000-word compressed reference the parent re-reads after every step-runner DONE. It contains:

1. **Current step tracker** — `Step N in progress; last DONE: step-X-runner; next: step-Y-runner`
2. **Hard caps** — 5 passes per reason per chapter, 3 rewrites per chapter, 4 critical reviewer passes, 3 stitch rounds, 10 total audit rounds, 3 Step 8 rounds
3. **Forbidden tokens list** — copy from Section 6.1
4. **Convergence criteria** — one-paragraph summary of Section 10.2
5. **Step-runner template paths** — the 11 paths listed in 5.5.4
6. **Worker template paths** — the 15 paths listed in 5.5.4
7. **Key directory paths** — manifest, convergence.log, chapters, audits, her-excerpts, templates
8. **Parallelism caps** — 2 step-1-batch-runners in flight, 2 step-3-chapter-runners in flight, 3 critical runners in flight, 1 stitch runner, 1 final runner

Each step-runner updates `parent-state.md`'s `current step tracker` field before returning DONE. The parent re-reads `parent-state.md` after each DONE to see the new state.

#### 5.5.6 Compaction is a fallback, not a routine

Because the parent is a pure dispatcher (Section 5.5.1–5.5.3), compaction should NOT be needed in normal operation. If the parent's context rises above the thresholds below, it is a signal that a rail was broken (parent read something forbidden, or a step-runner returned prose instead of a STATUS line).

| Context usage | Meaning | Action |
|---|---|---|
| < 25% | Normal. Parent is behaving as a thin dispatcher. | Continue. |
| 25–40% | Slight drift. Check convergence.log tail for recent unusual reads. | Continue but watch closely. |
| 40–55% | **Rail broken.** Something is bloating. | Stop dispatching. Read parent-state.md, identify which step is in flight. Compact with the preservation prompt below. After compaction, resume. |
| 55–75% | Serious drift. | Compact immediately with the preservation prompt. Investigate what broke. |
| > 75% | Emergency. | Compact. If it does not drop below 40%, dispatch a sub-orchestrator and let the current parent retire. |

**The compaction preservation prompt** (parent uses this exact text when invoking `/compact`, but should almost never need to):

```
/compact Preserve: (1) my role as the thin orchestrator of book-implementation-plan.md per Section 5.5; (2) the current step from .book-build/parent-state.md; (3) the paths .book-build/parent-state.md, .book-build/manifest.json, .book-build/convergence.log; (4) any in-flight step-runner dispatch IDs. Discard: (a) all contents of any source files or chapter files I may have read, (b) all stdout from Bash calls older than the last 60 seconds, (c) all tool-search and skill-invocation transcripts, (d) all prose from completed subagent results — keep only their STATUS JSON lines for in-flight dispatches. After compaction my first action is Read .book-build/parent-state.md, then Read .book-build/manifest.json.
```

#### 5.5.7 Subagent output suppression (STATUS-only final responses)

Every step-runner and every worker subagent's final response is **exactly one line** — the `STATUS: {...}` JSON. No prose, no narration, no tool-call echoes, no markdown headings, nothing before or after. All other output goes to files on disk. Section 8.0 codifies this; Sections 8.1–8.16 implement it.

This is non-negotiable because the Agent tool returns the subagent's final message verbatim to the parent, and any extra bytes bloat the parent's context. A step-runner that writes "I have completed Step 1 batch 3. All chapters drafted successfully. STATUS: {...}" gives the parent ~80 extra tokens per dispatch. Across 100 dispatches that's 8,000 tokens — 4% of the parent's budget, for no value.

### 5.6 Hierarchical execution model (parent → step-runner → worker)

The parent dispatches only **step-runner subagents**. Step-runners dispatch **worker subagents** (writers, auditors, revisers, stitch-auditor). Workers do the actual file work. This three-level hierarchy keeps the parent's context flat regardless of how many workers run in total.

```
PARENT (thin dispatcher, <15% context throughout)
  │
  ├─ dispatch → step-0-runner         → DONE
  ├─ dispatch → step-0.5-runner       → DONE  (writes parent-state.md, templates/)
  ├─ dispatch → step-0.9-runner       → PASS|FAIL  (verification gate; HALT on FAIL)
  │
  ├─ for each batch (15 batches: 14 × 4 chapters + 1 × 1 chapter):
  │    dispatch → step-1-batch-runner   → BATCH_DONE  (internally dispatches up to 4 writers)
  │
  ├─ dispatch → step-2-runner         → DONE  (internally dispatches 1 frontmatter + 10 part-intro writers)
  │
  ├─ for each chapter (57 chapters, 2 in flight):
  │    dispatch → step-3-chapter-runner → CHAPTER_AUDITED  (internally dispatches 6 auditors)
  │
  ├─ for each chapter needing revision:
  │    dispatch → step-4-revision-runner → DONE  (internally dispatches reviser or fresh writer)
  │
  ├─ dispatch → step-5-critical-runner  → DONE  (internally dispatches 3 critical reviewers)
  │
  ├─ dispatch → step-6-stitch-runner    → DONE  (internally dispatches 1 stitch auditor)
  │
  ├─ dispatch → step-7-backmatter-runner → DONE  (internally dispatches 3 back-matter writers)
  │
  └─ dispatch → step-8-final-runner     → DONE  (assembles final file, runs sanity checks)
```

**Rules**:
- Parent NEVER dispatches a writer, auditor, reviser, or stitch-auditor directly. Only step-runners.
- Step-runners NEVER dispatch other step-runners. The parent owns step-runner dispatch.
- A step-runner dispatches its workers in parallel or serially as its template specifies.
- Step-runners are fresh on every dispatch. They start from scratch, read `parent-state.md` + `manifest.json`, execute their step, update files, return DONE.
- The step-runner template is read FROM DISK by the step-runner itself, not embedded in the parent's dispatch prompt. Parent passes only the template path and the step-specific parameters (e.g., batch number).

**Why this works**: the parent's context grows by ~500 tokens per step-runner dispatch. Even with 14 Step 1 batches, 57 Step 3 chapter runs, 4 Step 5 critical rounds, and ~30 Step 4 revisions — totaling ~110 step-runner dispatches — the parent accumulates only ~55k tokens of dispatch history, well under its budget. No compaction needed.

---

## 6. Chapter Outline (57 chapters in 10 parts + 3 appendices)

### 6.1 Structural rules for every chapter (inherited unless overridden)
- **Target length**: 3,000–6,000 words of body (excluding mermaid code blocks and quoted code snippets)
- **Minimum per chapter**: 2 mermaid diagrams, 6 exact `path:Lnnn` citations, **4 quoted code snippets** from the listed source files, 2 HER cross-references, 1 explicit "where cc diverges from the published pattern" callout
- **Mandatory sections inside every chapter** (in order):
  1. `## Overview` (1-2 paragraphs stating the subsystem and why it matters)
  2. `## Data structures and contracts` (types, schemas, invariants — **must include at least 1 quoted code snippet showing a type/schema definition from the codebase**)
  3. `## Control flow` (how the subsystem runs; include mermaid; **must include at least 1 quoted code snippet showing a key function body from the codebase**)
  4. `## Edge cases and failure modes` (tie to HER failure-mode catalogue; quote relevant code where it materially clarifies the edge case)
  5. `## Where cc diverges from the published pattern` (explicit comparative analysis)
  6. `## Developer takeaways for building a long-running agent` (150–300 words — the reader's "what to do with this" summary)
- **Citation format**: `src/path/to/file.ts:L123` or `src/path/to/file.ts:L123-L145` for ranges. Inline with backticks.
- **Quoted code snippet format**: Every snippet is a fenced code block tagged with the correct language (`typescript`, `tsx`, `javascript`, `json`, `bash`, `yaml`) and the **first line is a comment** identifying the source:
  ```typescript
  // src/query.ts:L341-L389 — queryLoop async generator entry
  export async function* queryLoop(params: QueryParams): AsyncGenerator<...> {
    // ... verbatim code from the file ...
  }
  ```
  Rules for snippets:
  - Each snippet must be **verbatim** from the source file (whitespace and identifiers preserved). Writers do **not** paraphrase code; they copy the relevant lines.
  - Snippets may be **trimmed** with `// ...` markers for brevity (e.g., showing 15 lines out of a 60-line function), but the kept lines must be literal.
  - Every snippet must be **immediately followed** by 2–6 sentences of explanation that reference specific identifiers from the snippet (e.g., "The `deps.callModel()` call at line L352 is where …").
  - Snippets must average **8–30 lines each**. Snippets ≥ 50 lines are forbidden (split or trim).
  - A chapter may include **larger whole-block quotes** for type/schema definitions (e.g., a `z.object({...})` literal) when cutting would lose meaning; those are exempt from the 30-line cap but still capped at 60.
- **Diagram subset**: only `flowchart`, `sequenceDiagram`, `stateDiagram-v2`, `classDiagram`, `erDiagram` are allowed. (Simplifies pattern-based syntax validation.)
- **Forbidden tokens** (auto-revision trigger if found): `TODO`, `TBD`, `XXX`, `[NEEDS-VERIFY]`, `similar to chapter`, `as noted earlier`, `we will discuss`, `as we shall see`, `simply`, `obviously`, `just` (as filler), `in summary` (as a phrase — `## Summary` heading is fine)
- **Forward references** permitted but must be to chapters that exist in the manifest; must be resolved in Step 6 stitch audit

### 6.2 Part structure
- **Part I**: Foundations and Framing — Chapters 1–4
- **Part II**: Entry, Bootstrap, and the Runtime Spine — Chapters 5–10
- **Part III**: The Tool System — Chapters 11–17
- **Part IV**: Sub-Agents, Tasks, and Multi-Agent Dispatch — Chapters 18–24
- **Part V**: Context, Memory, and State — Chapters 25–31
- **Part VI**: Safety, Permissions, and Hooks — Chapters 32–37
- **Part VII**: Interaction Surfaces (Skills, Slash, MCP, UX) — Chapters 38–44
- **Part VIII**: Long-Running Work — Chapters 45–49
- **Part IX**: Observability, Cost, and Security — Chapters 50–52
- **Part X**: Synthesis and Future Development — Chapters 53–57
- **Appendices**: A, B, C

### 6.3 Chapter table (copy verbatim into `manifest.json`)

> For each chapter: `id`, `title`, `synopsis`, `source_files` (must all exist in repo; parent verifies at Step 0), `her_refs` (exact HER section identifiers), `diagram_reqs` (type + topic), `target_words`.

#### Part I — Foundations and Framing

**Ch 1. Why This Book Exists: The Harness Engineering Moment**
- Synopsis: Positions cc in the emerging discipline of harness engineering. States the book's thesis: cc is the most mature public example of a long-running agent harness and a template for trustworthy autonomous systems.
- Source files: `src/main.tsx`, `src/entrypoints/cli.tsx`, `README.md`
- HER refs: §1 "Agent = Model + Harness", §2 Evolution prompt→context→harness, §17 TerminalBench 2.0 24.9pp spread
- Diagrams: (a) flowchart "model + harness = agent" block diagram; (b) timeline flowchart METR task-duration doubling
- Target words: 3,000

**Ch 2. Book Architecture, Reading Paths, and Citation Conventions**
- Synopsis: Explains how to read the book, citation format, diagram conventions, cross-ref table from HER patterns/failure modes/best practices to chapters.
- Source files: (meta-chapter, references only)
- HER refs: §5 12 patterns index, §6 17 failure modes index, §20 12 best practices
- Diagrams: (a) classDiagram of the book's parts and dependencies; (b) flowchart mapping HER patterns to chapter numbers
- Target words: 3,000

**Ch 3. A Guided Tour of the Repository**
- Synopsis: Maps the top-level directory tree. For each major folder answers: what lives here, why, connections to neighbors. Reader's orientation map before deep dives.
- Source files: `package.json`, `tsconfig.json`, `bunfig.toml`, `src/main.tsx` (imports only), directory listing of `src/` depth 2
- HER refs: §21 "Reference Architecture 8 Layers" mapped onto cc folders
- Diagrams: (a) flowchart directory dependency graph; (b) classDiagram of top-level modules; (c) flowchart mapping HER 8-layer reference architecture onto cc folders
- Target words: 5,000

**Ch 4. TypeScript, Bun, React, Ink: The Unusual Runtime Stack**
- Synopsis: Why cc is TS+Bun+React+Ink. Tradeoffs (startup time, distributability, React for structured terminal UI, feature-flagged bundling). Connects to HER's thesis that harnesses are language/runtime-specific.
- Source files: `package.json`, `tsconfig.json`, `bunfig.toml`, `src/ink/ink.tsx`, `src/ink/screen.ts`, `src/utils/bundledMode.ts` (or nearest equivalent)
- HER refs: §4 Fowler taxonomy, §12 supply chain
- Diagrams: (a) flowchart runtime stack Bun → main.tsx → Ink tree → terminal; (b) stateDiagram-v2 for bundle modes
- Target words: 3,500

#### Part II — Entry, Bootstrap, and the Runtime Spine

**Ch 5. Bootstrap: From `bun run` to Running Loop**
- Synopsis: Startup from `cli.tsx` through fast-path flags, MDM prefetch, keychain prefetch, version check, TLS, policy limits, mTLS, OAuth, graceful shutdown registration.
- Source files: `src/entrypoints/cli.tsx`, `src/entrypoints/init.ts`, `src/bootstrap/state.ts`, `src/utils/settings/settings.ts`
- HER refs: §10 Session Protocol (ORIENT/SETUP), §12 TLS/mTLS defense in depth
- Diagrams: (a) sequenceDiagram cli.tsx → init.ts → main.tsx with every side-effect labeled; (b) stateDiagram-v2 for the boot state machine
- Target words: 4,500

**Ch 6. `main.tsx` and the Command Router**
- Synopsis: Deep dive into the ~4,700 LOC `main.tsx`. CLI subcommands, parsing, `renderAndRun`, bifurcation between headless and interactive modes.
- Source files: `src/main.tsx`
- HER refs: §7 Configuration Surfaces (entry flags as a surface)
- Diagrams: (a) flowchart of command routing decisions; (b) sequenceDiagram headless vs interactive launch
- Target words: 4,500

**Ch 7. The Query Loop: Heartbeat of the Agent**
- Synopsis: Exhaustive walkthrough of `queryLoop()` in `src/query.ts`. Async generator protocol, StreamEvent vs Message types, tool dispatch points, interruption points, how the loop bridges model streaming and tool execution.
- Source files: `src/query.ts`, `src/QueryEngine.ts`, `src/query/config.ts`, `src/query/deps.ts`, `src/query/stopHooks.ts`, `src/query/tokenBudget.ts`
- HER refs: §5 Pattern 6 "Explore-Plan-Act Loop", §6.1 Context Rot, §6.2 Premature Completion, §6.7 Infinite Loops
- Diagrams: (a) sequenceDiagram of a full query iteration; (b) stateDiagram-v2 of query loop states; (c) flowchart of stop-hook evaluation and token budget checks
- Target words: 6,000

**Ch 8. Talking to Anthropic: `services/api/claude.ts` and Streaming**
- Synopsis: Anthropic SDK wrapper. Streaming, fallback model, token metering, retries, prompt cache break detection, rate limits, first-token date, usage accumulation.
- Source files: `src/services/api/claude.ts`, `src/services/api/usage.ts`, `src/services/api/errors.ts`, `src/services/api/logging.ts`
- HER refs: §13 cost management (observation masking), §6.14 Model regression from provider updates
- Diagrams: (a) sequenceDiagram of a streaming request with retry; (b) stateDiagram-v2 of request lifecycle including fallback; (c) flowchart of usage accounting and cost hooks
- Target words: 5,000

**Ch 9. System Prompts and Prompt Assembly**
- Synopsis: `src/constants/prompts.ts` (~900 LOC) and how cc composes the final system prompt per query from base, environment, tools, skills, memories, hooks, effort, plan mode.
- Source files: `src/constants/prompts.ts`, `src/utils/analyzeContext.ts`, `src/utils/claudemd.ts`, `src/utils/context.ts`
- HER refs: §7.1 Persistent Instruction File, §5 Pattern 2 Scoped Context Assembly, §7.1 ETH Zurich AGENTS.md finding (context files can HURT performance by >20% cost)
- Diagrams: (a) flowchart of prompt assembly layers (base → env → tools → memories → skills → hooks → CLAUDE.md); (b) classDiagram of prompt fragment sources
- Target words: 5,000

**Ch 10. Token Budgets, Effort, and Fast Mode**
- Synopsis: How cc tracks and bounds token consumption, how "effort" selects model/thinking, how fast mode flips the loop.
- Source files: `src/query/tokenBudget.ts`, `src/services/tokenEstimation.ts`, `src/state/AppStateStore.ts`
- HER refs: §13 Cost and Budgeting, §9.4 Model Routing (Opus/Sonnet/Haiku tiers)
- Diagrams: (a) stateDiagram-v2 for effort levels; (b) flowchart of token-budget-triggered actions
- Target words: 3,500

#### Part III — The Tool System

**Ch 11. Anatomy of a Tool: `Tool.ts`, `buildTool`, and the Base Type**
- Synopsis: The tool contract — Zod/JSONSchema input, `call()`, concurrency flags, deferral, max result size. Walks through `buildTool` helper.
- Source files: `src/Tool.ts`, `src/tools.ts`
- HER refs: §5 Pattern 11 Single-Purpose Tool Design, §4 Fowler computational vs inferential
- Diagrams: (a) classDiagram of the `Tool` interface and concrete implementations; (b) flowchart of `buildTool()` behavior
- Target words: 4,000

**Ch 12. Tool Dispatch Pipeline**
- Synopsis: How a requested tool call travels: `validateInput → canUseTool → runPreToolUseHooks → tool.call → runPostToolUseHooks → result normalization`. Read-only parallelism and write serialization.
- Source files: `src/services/tools/toolExecution.ts`, `src/services/tools/toolOrchestration.ts`, `src/services/tools/toolHooks.ts`, `src/services/tools/StreamingToolExecutor.ts`, `src/hooks/useCanUseTool.tsx`
- HER refs: §5 Pattern 12 Deterministic Lifecycle Hooks, §6.8 Tool Explosion, §6.6 Silent Failures
- Diagrams: (a) sequenceDiagram of a successful tool dispatch; (b) sequenceDiagram of a denied dispatch; (c) stateDiagram-v2 of deferral / concurrency scheduling
- Target words: 5,500

**Ch 13. File System Tools: Read, Write, Edit, Glob, Grep, Notebook**
- Synopsis: File tools, safety checks, multi-edit semantics, image/pdf/notebook support, glob/grep semantics, relationship to file-history snapshots.
- Source files: `src/tools/FileReadTool/`, `src/tools/FileWriteTool/`, `src/tools/FileEditTool/`, `src/tools/GlobTool/`, `src/tools/GrepTool/`, `src/tools/NotebookEditTool/`, `src/utils/fileHistory.ts`
- HER refs: §5 Pattern 11, §6.4 Placeholder Implementations, §6.15 Data Leakage Between Contexts
- Diagrams: (a) sequenceDiagram of Edit tool with pre-read and snapshot; (b) classDiagram of the file-tool family
- Target words: 5,000

**Ch 14. The Bash Tool, Classifiers, and Sandboxing**
- Synopsis: `BashTool` and its safety subsystem. Command classification, destructive patterns, mode validation, sandbox detection, PowerShell variant.
- Source files: `src/tools/BashTool/`, `src/tools/PowerShellTool/`, `src/utils/permissions/bashClassifier.ts`
- HER refs: §5 Pattern 10 Command Risk Classification, §6.16 Checkpoint-Restore Side Effects, §12.4 Security Threat Model
- Diagrams: (a) flowchart of a Bash command's risk classification; (b) classDiagram of the classifier pipeline; (c) stateDiagram-v2 for sandbox / permission decisions
- Target words: 6,000

**Ch 15. Search, LSP, and Code Analysis Tools**
- Synopsis: `GrepTool`, `LSPTool`, `ToolSearchTool`. Progressive tool disclosure and why it matters for large catalogs.
- Source files: `src/tools/LSPTool/`, `src/tools/ToolSearchTool/`, `src/services/lsp/`
- HER refs: §5 Pattern 9 Progressive Tool Expansion, §6.8 Tool Explosion
- Diagrams: (a) sequenceDiagram of ToolSearch → deferred tool → first call; (b) classDiagram of LSP client lifecycle
- Target words: 4,000

**Ch 16. Web Tools: `WebFetch`, `WebSearch`, and SSRF Defense**
- Synopsis: Web tool implementations, SSRF guard, caching, how web access is gated. Ties to prompt-injection threat model.
- Source files: `src/tools/WebFetchTool/`, `src/tools/WebSearchTool/`, `src/utils/hooks/ssrfGuard.ts`
- HER refs: §6.13 Prompt Injection, §6.12 Hallucinated Tool Calls
- Diagrams: (a) flowchart of a `WebFetch` call with SSRF guard; (b) sequenceDiagram of web-content sanitization feeding into the model
- Target words: 3,500

**Ch 17. TodoWrite, AskUserQuestion, Brief, Config, SendMessage**
- Synopsis: The "meta" tools that structure sessions: TodoWrite (self-planning), AskUserQuestion (HITL), Brief, Config, SendMessage (teammate comms).
- Source files: `src/tools/TodoWriteTool/`, `src/tools/AskUserQuestionTool/`, `src/tools/BriefTool/`, `src/tools/ConfigTool/`, `src/tools/SendMessageTool/`
- HER refs: §11 Human-in-the-Loop Design Patterns, §5 Pattern 5 Progressive Context Compaction
- Diagrams: (a) stateDiagram-v2 of a todo item lifecycle; (b) sequenceDiagram of `AskUserQuestion` interrupting the loop
- Target words: 4,000

#### Part IV — Sub-Agents, Tasks, and Multi-Agent Dispatch

**Ch 18. The Agent Tool: Entry, Inputs, and Lifecycle**
- Synopsis: `AgentTool` surface. Inputs (description, prompt, subagent_type, model, run_in_background, team_name, mode, isolation, cwd), validation, output return.
- Source files: `src/tools/AgentTool/AgentTool.tsx`
- HER refs: §7.4 Sub-Agents as a configuration surface, §5 Pattern 7 Context-Isolated Subagents, §5 Pattern 8 Fork-Join Parallelism
- Diagrams: (a) classDiagram of `AgentTool` input schema; (b) flowchart of AgentTool input validation and dispatch selection
- Target words: 5,000

**Ch 19. Sync vs Fork vs Remote: The Execution Modes**
- Synopsis: In-process sync agents, forked subagents, remote CCR agents. Tradeoffs, shared vs isolated state, communication paths.
- Source files: `src/tools/AgentTool/runAgent.ts`, `src/tools/AgentTool/forkSubagent.ts`, `src/utils/forkedAgent.ts`, `src/tasks/RemoteAgentTask/`, `src/tasks/LocalAgentTask/`
- HER refs: §9.1 Three Tiers of Multi-Agent Orchestration, §6.5 Context Anxiety
- Diagrams: (a) sequenceDiagram for sync mode; (b) sequenceDiagram for fork mode; (c) sequenceDiagram for remote mode
- Target words: 5,500

**Ch 20. Agent Definitions: Loading, Frontmatter, and Built-Ins**
- Synopsis: How cc discovers and loads agents from `~/.claude/agents`, plugin agents, built-ins. Markdown + YAML frontmatter contract.
- Source files: `src/tools/AgentTool/loadAgentsDir.ts`, `src/utils/plugins/loadPluginAgents.ts`, `src/utils/frontmatterParser.ts`
- HER refs: §7.4 Sub-agents configuration surface, §5 Pattern 2 Scoped Context Assembly
- Diagrams: (a) flowchart of agent discovery across directories; (b) classDiagram of AgentDefinition shape
- Target words: 4,000

**Ch 21. Multi-Agent Coordinator: The Swarm Layer**
- Synopsis: The feature-gated `coordinator/coordinatorMode.ts` — how multi-agent swarms are orchestrated, message routing, team lifecycle.
- Source files: `src/coordinator/coordinatorMode.ts`
- HER refs: §9.2 Generator-Evaluator pattern and Ouroboros problem, §9.3 Agent Communication (file-based), §5 Pattern 8
- Diagrams: (a) classDiagram of coordinator structures; (b) sequenceDiagram of team creation and message routing; (c) stateDiagram-v2 of team lifecycle
- Target words: 6,000

**Ch 22. Tasks: A Durable Unit of Work**
- Synopsis: Task records, status transitions, blocking relationships, filesystem locking, storage paths, high-water mark, the seven task types.
- Source files: `src/utils/tasks.ts`, `src/tasks/LocalAgentTask/`, `src/tasks/RemoteAgentTask/`, `src/tasks/DreamTask/`, `src/tasks/LocalMainSessionTask.ts`, `src/tools/TaskCreateTool/`, `src/tools/TaskListTool/`, `src/tools/TaskGetTool/`, `src/tools/TaskUpdateTool/`, `src/tools/TaskStopTool/`, `src/tools/TaskOutputTool/`
- HER refs: §10.1 One-Task-Per-Session rule, §8.2 Three-File State Pattern, §10.3 Session Protocol
- Diagrams: (a) stateDiagram-v2 of task status transitions; (b) classDiagram of the seven task types; (c) sequenceDiagram of task create/lock/update/complete
- Target words: 6,000

**Ch 23. Teammates and In-Process Collaboration**
- Synopsis: tmux-based teammates, in-process teammate tasks, `SendMessageTool`, idle hooks.
- Source files: `src/tools/SendMessageTool/`, `src/utils/hooks/sessionHooks.ts` (TeammateIdle event)
- HER refs: §9.1 in-process tier, §11.5 Handoff Protocols
- Diagrams: (a) sequenceDiagram of a teammate message exchange; (b) stateDiagram-v2 of teammate life (active/idle/shutdown)
- Target words: 4,000

**Ch 24. Dream Tasks: Background Consolidation**
- Synopsis: `DreamTask` and `autoDream` — background memory consolidation loop, firing conditions, interaction with memories.
- Source files: `src/tasks/DreamTask/`, `src/services/autoDream/autoDream.ts`, `src/services/autoDream/consolidationLock.ts`, `src/services/autoDream/consolidationPrompt.ts`
- HER refs: §5 Pattern 4 Dream Consolidation, §21 Layer 8 Learning and Adaptation
- Diagrams: (a) stateDiagram-v2 of the dream lifecycle; (b) sequenceDiagram of consolidation with lock
- Target words: 4,500

#### Part V — Context, Memory, and State

**Ch 25. Messages and the Conversation Model**
- Synopsis: `src/utils/messages.ts` and `src/types/message.ts` — message types, `<history_snip>`, compact boundaries, attachments, content replacement.
- Source files: `src/utils/messages.ts`, `src/types/message.ts`, `src/utils/attachments.ts`
- HER refs: §5 Pattern 5, §6.1 Context Rot
- Diagrams: (a) classDiagram of message types; (b) flowchart of message lifecycle
- Target words: 4,500

**Ch 26. Memdir: Tiered Memory on Disk**
- Synopsis: Memdir architecture. MEMORY.md index, per-memory `.md` files, types (user/feedback/project/reference), age, scan, relevance retrieval.
- Source files: `src/memdir/memdir.ts`, `src/memdir/memoryTypes.ts`, `src/memdir/paths.ts`, `src/memdir/findRelevantMemories.ts`, `src/memdir/memoryScan.ts`, `src/memdir/memoryAge.ts`
- HER refs: §5 Pattern 3 Tiered Memory, §7.1 Persistent Instruction File
- Diagrams: (a) erDiagram of memdir storage; (b) flowchart of memory retrieval per query; (c) classDiagram of memory types and frontmatter
- Target words: 6,000

**Ch 27. Session Memory and Memory Extraction**
- Synopsis: `services/SessionMemory/` and memory extraction pipelines, prompts that drive it, feature gates.
- Source files: `src/services/SessionMemory/sessionMemory.ts`, `src/services/SessionMemory/prompts.ts`
- HER refs: §5 Pattern 3 Tiered Memory, §6.3 Self-Evaluation Bias
- Diagrams: (a) sequenceDiagram of memory extraction after a session; (b) flowchart of memory promotion session → memdir
- Target words: 4,500

**Ch 28. Compaction Hierarchy: Five Stages of Context Rescue**
- Synopsis: `<history_snip>` → Microcompact → Context Collapse → Autocompact → Hard Reset. Triggers, preservation semantics, discard semantics.
- Source files: `src/services/compact/compact.ts`, `src/services/compact/autoCompact.ts`, `src/services/compact/reactiveCompact.ts`, `src/services/compact/microCompact.ts`, `src/services/compact/cachedMicrocompact.ts`, `src/services/compact/timeBasedMCConfig.ts`, `src/services/compact/postCompactCleanup.ts`, `src/services/contextCollapse/` (if present), `src/utils/messages.ts`
- HER refs: §5 Pattern 5 Progressive Context Compaction, §8 Context Management, §8.1 Observation Masking (52% cost reduction), §6.1 Context Rot
- Diagrams: (a) flowchart of the five-stage hierarchy; (b) stateDiagram-v2 of compaction triggers; (c) sequenceDiagram of a microcompact pass
- Target words: 6,000

**Ch 29. Session Persistence and Resume**
- Synopsis: JSONL sessions, entry types, tombstones, head/tail reading, resume flow.
- Source files: `src/utils/sessionStorage.ts`, `src/utils/sessionStoragePortable.ts`
- HER refs: §6.16 Checkpoint-Restore Side Effects (ACRFence), §10.3 Session Protocol
- Diagrams: (a) erDiagram of session storage layout; (b) stateDiagram-v2 of a session's persistence lifecycle; (c) sequenceDiagram of a resume flow
- Target words: 5,500

**Ch 30. AppState: The Redux-like Store**
- Synopsis: `AppState.tsx`, `AppStateStore.ts`, `onChangeAppState.ts` — shape, reducers, subscribers, teamContext.
- Source files: `src/state/AppState.tsx`, `src/state/AppStateStore.ts`, `src/state/onChangeAppState.ts`, `src/bootstrap/state.ts`
- HER refs: §21 Layer 2 Session Management
- Diagrams: (a) classDiagram of store shape; (b) sequenceDiagram of a state change propagating to subscribers
- Target words: 4,000

**Ch 31. CLAUDE.md, Settings Cascade, and Managed Settings**
- Synopsis: Loading order (org → user → project → dir → env → flags), managed settings for enterprise, CLAUDE.md discovery and role.
- Source files: `src/utils/settings/settings.ts`, `src/utils/settings/types.ts`, `src/utils/settings/settingsCache.ts`, `src/utils/claudemd.ts`
- HER refs: §7.1 CLAUDE.md / AGENTS.md, §7 Six Configuration Surfaces
- Diagrams: (a) flowchart of settings cascade precedence; (b) classDiagram of settings sources
- Target words: 4,500

#### Part VI — Safety, Permissions, and Hooks

**Ch 32. The Permission Model: Modes and Rules**
- Synopsis: Permission modes (default, plan, acceptEdits, bypassPermissions, dontAsk, auto). Rule parsing and evaluation.
- Source files: `src/utils/permissions/permissions.ts`, `src/utils/permissions/PermissionMode.ts`, `src/utils/permissions/PermissionRule.ts`
- HER refs: §12 Guardrails, §11 Human-in-the-Loop, §6.13 Prompt Injection
- Diagrams: (a) stateDiagram-v2 of permission modes; (b) flowchart of rule evaluation; (c) classDiagram of rule types
- Target words: 5,500

**Ch 33. Bash Classifier and YOLO Scoring**
- Synopsis: `bashClassifier.ts`, yoloClassifier, classifier approvals. How commands are mapped to risk bands and how auto mode uses classification.
- Source files: `src/utils/permissions/bashClassifier.ts`
- HER refs: §5 Pattern 10 Command Risk Classification, §12.4 Security Threat Model
- Diagrams: (a) flowchart of the classifier decision tree; (b) classDiagram of classifier inputs
- Target words: 5,000

**Ch 34. Filesystem Permissions and Path Guards**
- Synopsis: `filesystem.ts`, symlink traversal defense, glob allow/deny, denial tracking.
- Source files: `src/utils/permissions/filesystem.ts`, `src/utils/permissions/denialTracking.ts`
- HER refs: §12.4 Data Exfiltration, §6.15 Data Leakage
- Diagrams: (a) flowchart of path validation; (b) stateDiagram-v2 of denial tracking
- Target words: 4,000

**Ch 35. `useCanUseTool`: The Engine Hook**
- Synopsis: The ~200 LOC React hook that evaluates permission decisions, interacts with UI prompts, enforces plan mode.
- Source files: `src/hooks/useCanUseTool.tsx`
- HER refs: §11.2 Tiered Escalation, §11.3 Async Approval
- Diagrams: (a) sequenceDiagram of the hook responding to a tool request; (b) stateDiagram-v2 of the approval UI; (c) flowchart of fallback decisions when no rule matches
- Target words: 5,500

**Ch 36. The Hook Schema and Lifecycle Events**
- Synopsis: Hook schema in `schemas/hooks.ts`. Each event documented with trigger, payload, effect. Hook types: command, prompt, http, agent, function. Exit code semantics.
- Source files: `src/schemas/hooks.ts`, `src/utils/hooks.ts`, `src/utils/hooks/hooksSettings.ts`, `src/utils/hooks/hooksConfigManager.ts`
- HER refs: §5 Pattern 12 Deterministic Lifecycle Hooks, §7 Configuration Surfaces
- Diagrams: (a) classDiagram of hook schema; (b) erDiagram of all events with triggers; (c) flowchart of exit-code handling
- Target words: 6,000

**Ch 37. Hook Execution: Command, Prompt, HTTP, Agent, Function**
- Synopsis: Execution pipelines per hook type, registry, async hook registry, SSRF guard, session hooks.
- Source files: `src/utils/hooks/execPromptHook.ts`, `src/utils/hooks/execAgentHook.ts`, `src/utils/hooks/execHttpHook.ts`, `src/utils/hooks/sessionHooks.ts`, `src/utils/hooks/registerSkillHooks.ts`, `src/utils/hooks/hookEvents.ts`, `src/utils/hooks/ssrfGuard.ts`
- HER refs: §5 Pattern 12, §12.4 Supply Chain Attacks via MCP/Skills/Hooks
- Diagrams: (a) sequenceDiagram of a prompt hook; (b) sequenceDiagram of an HTTP hook with SSRF check
- Target words: 5,000

#### Part VII — Interaction Surfaces: Skills, Slash, MCP, UX

**Ch 38. Skills: Discovery, Frontmatter, Progressive Disclosure**
- Synopsis: Skill directories, bundled skills, frontmatter fields, user-invocable vs triggered, inline vs fork context.
- Source files: `src/skills/loadSkillsDir.ts`, `src/skills/bundledSkills.ts`, `src/tools/SkillTool/SkillTool.ts`
- HER refs: §7.3 Skills (Progressive Disclosure), §5 Pattern 9, §12.4 Supply Chain via Skills
- Diagrams: (a) flowchart of skill discovery; (b) stateDiagram-v2 of skill invocation modes (inline/fork); (c) classDiagram of skill frontmatter
- Target words: 5,500

**Ch 39. Slash Commands: 90+ Commands and the Registry**
- Synopsis: `commands.ts` registry, prompt vs callback types, user-defined commands, argument substitution.
- Source files: `src/commands.ts`, representative selection from `src/commands/` (plan, compact, memory, agents, hooks, mcp, doctor, review, ultraplan), `src/utils/processUserInput/processSlashCommand.tsx`, `src/utils/slashCommandParsing.ts`
- HER refs: §7 Configuration Surfaces
- Diagrams: (a) classDiagram of slash-command types; (b) sequenceDiagram of slash-command parsing and dispatch
- Target words: 5,500

**Ch 40. MCP: Clients, Transports, and Lifecycle**
- Synopsis: MCP transports, server lifecycle, reconnect strategy, OAuth, elicitation handling.
- Source files: `src/services/mcp/client.ts`, `src/services/mcp/config.ts`, `src/services/mcp/auth.ts`, `src/services/mcp/types.ts`, `src/services/mcp/MCPConnectionManager.tsx`, `src/entrypoints/mcp.ts`
- HER refs: §7.2 MCP Servers, §12.4 Supply Chain via MCP, §6.15 Data Leakage
- Diagrams: (a) classDiagram of transports; (b) stateDiagram-v2 of server connection states; (c) sequenceDiagram of the OAuth flow
- Target words: 6,000

**Ch 41. MCP Tools: Passthrough Permissions and Deferred Loading**
- Synopsis: `MCPTool`, `ListMcpResourcesTool`, `ReadMcpResourceTool`, `McpAuthTool`. Why MCP tools are deferred and how ToolSearch surfaces them.
- Source files: `src/tools/MCPTool/MCPTool.ts`, `src/tools/ListMcpResourcesTool/`, `src/tools/ReadMcpResourceTool/`
- HER refs: §5 Pattern 9 Progressive Tool Expansion, §6.13 Prompt Injection via MCP responses
- Diagrams: (a) sequenceDiagram of a deferred MCP tool first call; (b) classDiagram of MCP tool wrapper
- Target words: 4,500

**Ch 42. The Ink Renderer and Terminal Engine**
- Synopsis: Custom React reconciler, Yoga layout, output rendering, focus, selection, ANSI handling.
- Source files: `src/ink/ink.tsx`, `src/ink/screen.ts`, `src/ink/output.ts`, `src/ink/Ansi.tsx`
- HER refs: §21 reference architecture UX layer
- Diagrams: (a) classDiagram of the Ink virtual DOM; (b) sequenceDiagram of a keypress → render cycle; (c) stateDiagram-v2 of focus management
- Target words: 6,000

**Ch 43. REPL, PromptInput, Typeahead, Voice, Keybindings**
- Synopsis: REPL screen, prompt composer, typeahead (~1,400 LOC hook), voice integration (~1,100 LOC hook), bridge/inbox, keybinding context.
- Source files: `src/screens/REPL.tsx`, `src/components/PromptInput/PromptInput.tsx`, `src/hooks/useTypeahead.tsx`, `src/hooks/useVoice.ts`, `src/hooks/useGlobalKeybindings.tsx`
- HER refs: §11 Human-in-the-Loop, §7 Configuration Surfaces
- Diagrams: (a) classDiagram of REPL components; (b) sequenceDiagram of a prompt from keystroke to model send; (c) stateDiagram-v2 of voice capture
- Target words: 6,000

**Ch 44. Bridges: VS Code, JetBrains, and Web**
- Synopsis: IDE bridge architecture, trusted devices, inbound attachments.
- Source files: `src/bridge/replBridge.ts`, `src/bridge/bridgeMain.ts`
- HER refs: §9.1 local orchestrators, §12.4 Data Exfiltration via bridge
- Diagrams: (a) classDiagram of bridge transport layers; (b) sequenceDiagram of a VS Code → cc → model round trip
- Target words: 5,000

#### Part VIII — Long-Running Work

**Ch 45. Plan Mode V2: The Five-Phase Model**
- Synopsis: Plan mode phases, parallel agent counts by subscription, read-only enforcement, plan storage.
- Source files: `src/tools/EnterPlanModeTool/EnterPlanModeTool.ts`, `src/tools/ExitPlanModeTool/ExitPlanModeV2Tool.ts`, `src/utils/planModeV2.ts`
- HER refs: §5 Pattern 6 Explore-Plan-Act Loop, §10.3 Session Protocol
- Diagrams: (a) stateDiagram-v2 of plan mode phases; (b) flowchart of plan-mode tool gating; (c) sequenceDiagram of parallel exploration agents
- Target words: 5,500

**Ch 46. Worktrees: Isolated Parallel Branches**
- Synopsis: Worktree creation, slug validation, symlinked node_modules, worktree hooks, enter/exit tools.
- Source files: `src/tools/EnterWorktreeTool/`, `src/tools/ExitWorktreeTool/`, `src/utils/worktree.ts`
- HER refs: §5 Pattern 8 Fork-Join Parallelism, §12.4 path traversal defense
- Diagrams: (a) erDiagram of worktree storage layout; (b) stateDiagram-v2 of worktree lifecycle
- Target words: 4,000

**Ch 47. Cron, Schedule, Loop, and Wakeup**
- Synopsis: Scheduled tasks, max 50/project, storage, dynamic loop sentinel, monitor tool.
- Source files: `src/tools/ScheduleCronTool/`, `src/utils/cronTasks.ts`, `src/utils/cron.ts`
- HER refs: §10.2 Task Sizing Research (METR), §10.1 One-Task-Per-Session
- Diagrams: (a) classDiagram of CronTask record; (b) stateDiagram-v2 of cron job lifecycle; (c) sequenceDiagram of loop-dynamic wakeup
- Target words: 5,000

**Ch 48. Remote Agents and CCR Integration**
- Synopsis: RemoteTriggerTool, remote session manager, websocket sessions, how CCR binds to the local parent.
- Source files: `src/tools/RemoteTriggerTool/`, `src/tasks/RemoteAgentTask/`
- HER refs: §9.1 cloud async tier, §6.16 Checkpoint-Restore
- Diagrams: (a) classDiagram of remote session; (b) sequenceDiagram of a remote trigger
- Target words: 5,000

**Ch 49. KAIROS Assistant, Buddy, and Experimental Layers**
- Synopsis: Proactive assistant mode, Tamagotchi easter egg, feature-gated experiments. Why they matter for UX warmth and observability.
- Source files: `src/assistant/index.ts`, `src/assistant/gate.ts`, `src/assistant/sessionHistory.ts`, `src/buddy/companion.ts`, `src/buddy/sprites.ts`
- HER refs: §11 Human-in-the-Loop affordances, §18 Over-trust risk
- Diagrams: (a) stateDiagram-v2 of Buddy stats (DEBUGGING/CHAOS/SNARK); (b) classDiagram of Buddy sprite/gacha
- Target words: 3,500

#### Part IX — Observability, Cost, and Security

**Ch 50. Analytics, Cost Tracking, and GrowthBook**
- Synopsis: Sink interface, datadog, growthbook feature flags, first-party logging, tengu_* events, cost tracker, PII markers.
- Source files: `src/services/analytics/index.ts`, `src/services/analytics/growthbook.js`, `src/utils/cost-tracker.ts`
- HER refs: §14 Observability (three pillars), §13 Cost Management and Budgeting
- Diagrams: (a) classDiagram of sinks; (b) flowchart of event flow from tool call to sink; (c) stateDiagram-v2 for kill-switches
- Target words: 5,500

**Ch 51. Debug Logs, Diagnostics, and Doctor**
- Synopsis: Debug logging, diag logs, doctor screen, security review command, perf issue collection.
- Source files: `src/utils/debug.ts`, `src/commands/doctor/` (or nearest equivalent)
- HER refs: §14.2 Observability Gap, §6.6 Silent Failures
- Diagrams: (a) classDiagram of logging layers; (b) sequenceDiagram of a doctor run
- Target words: 4,500

**Ch 52. Threat Model: CC Through the Lens of HER Section 12**
- Synopsis: Walks HER's five-layer defense-in-depth and maps each layer to cc's concrete defenses. Covers prompt injection, supply chain (MCP/skills/plugins), system-prompt leakage, data exfiltration, checkpoint-restore.
- Source files: `src/utils/permissions/`, `src/utils/hooks/ssrfGuard.ts`, `src/upstreamproxy/`
- HER refs: §12 Guardrails Safety and Security (full section), §6.11–6.17 failure modes
- Diagrams: (a) classDiagram of the five-layer defense; (b) flowchart of a prompt-injection scenario and mitigations; (c) erDiagram mapping HER threats to cc files
- Target words: 6,000

#### Part X — Synthesis and Future Development

**Ch 53. The 12 HER Patterns Mapped to CC**
- Synopsis: One section per pattern. For each: the pattern (HER), cc's implementation (with file paths), divergences, gaps, lessons.
- Source files: summary references back to earlier chapters (no new primary reads)
- HER refs: §5 all 12 patterns
- Diagrams: (a) erDiagram mapping 12 patterns to cc subsystems; (b) flowchart showing pattern interactions in a running query
- Target words: 6,000

**Ch 54. The 17 Failure Modes: How CC Defends (or Doesn't)**
- Synopsis: One subsection per failure mode. cc's defense (or absence of defense). Specific file/line evidence.
- Source files: summary references back to earlier chapters
- HER refs: §6 all 17 failure modes
- Diagrams: (a) erDiagram failure ↔ defense ↔ file; (b) flowchart of the failure-mode escalation ladder
- Target words: 6,000

**Ch 55. Reference Architecture: Building a Trustworthy Long-Running Agent**
- Synopsis: Using HER's 8-layer reference architecture and cc's concrete implementations as a guide, sketches the architecture of a future "launch and trust" coding agent: sub-agent dispatcher, context engineering, back-pressure, cost/observability, multi-agent coordination, adaptation.
- Source files: summary references
- HER refs: §21 Reference Architecture, §20 20 Best Practices, §19 Decision Framework
- Diagrams: (a) classDiagram of the 8 layers instantiated with cc components; (b) sequenceDiagram of a long-running "trust" session; (c) stateDiagram-v2 of an agent operating under strict back-pressure
- Target words: 6,000

**Ch 56. What To Build Next: An Opinionated Roadmap**
- Synopsis: Concrete roadmap for a team building on top of the cc codebase: what to keep, what to rewrite, what to add (proper quality gates, stronger HITL, dead-letter queues, evaluation harness, multi-hour dashboards).
- Source files: summary references
- HER refs: §11, §13, §14, §20
- Diagrams: (a) classDiagram of proposed additions; (b) flowchart of the new quality-gates pipeline
- Target words: 5,500

**Ch 57. Closing: Lessons for the Harness Engineering Discipline**
- Synopsis: Zoom out. Reflections on what cc proves (and disproves) about the discipline. Open research questions. Call to action.
- Source files: (reflective chapter; no new code reads)
- HER refs: §18 Limitations, §10.2 METR, §15 Academic Research (NLAH, Meta-Harness, AutoHarness, ACRFence)
- Diagrams: (a) flowchart of discipline maturity levels; (b) timeline flowchart of discipline evolution
- Target words: 4,000

#### Appendices

**App A — Source File Concordance** (2,500–4,000 words)
- Alphabetical index of every source file referenced in the book, mapped to the chapters that cite it. Generated from `manifest.json` at Step 7 by a dedicated back-matter subagent; then manually reviewed by the concordance auditor.

**App B — HER Cross-Reference Table** (2,000–3,000 words)
- Every HER section/pattern/failure-mode/best-practice mapped to the chapters that cross-reference it. Also generated at Step 7.

**App C — Glossary and Bibliography** (2,000–3,000 words)
- Glossary of cc terms, HER terms, academic terms derived from `terminology.json`. Bibliography copies HER's 60+ sources plus any cc-internal docs and README references.

### 6.4 Totals
- **57 chapters**, **3 appendices**
- Sum of chapter target lengths ≈ **270,000 words**
- Add front matter, 10 part intros, and back matter ≈ **285,000–295,000 words total**
- **Guard bounds**: final file must land in **[200,000, 310,000] words**; if under 200k, parent triggers a "depth" rewrite pass on the shortest chapters; if over 310k, parent triggers a "trim" pass on the longest chapters.

### 6.5 Canonical source file corrections (DO NOT re-discover these)

The chapter table in Section 6.3 references source paths derived from preliminary Explore-agent reports. Subsequent validation against the live repo (two runs' worth) produced the canonical corrections below. **The step-0-runner MUST apply these corrections to `manifest.json` at Step 0 as baseline — do not re-validate or re-discover.** If the repo evolves and additional files become missing, the step-0-runner logs them as `path_downgraded` events, but the corrections in this table are mandatory and not subject to re-verification.

#### 6.5.1 Files that DO NOT EXIST in the leaked repo (remove from their chapter's `source_files` entirely)

| File | Affected chapter(s) | Status | Action |
|---|---|---|---|
| `package.json` | Ch 3, Ch 4 | not in sourcemap leak | Remove; note "build config not in sourcemap leak" in chapter |
| `tsconfig.json` | Ch 3, Ch 4 | not in sourcemap leak | Remove; same note |
| `bunfig.toml` | Ch 4 | not in sourcemap leak | Remove; same note |
| `src/types/message.ts` | Ch 25 | does not exist | Remove; message types live in `src/utils/messages.ts` |
| `src/services/compact/reactiveCompact.ts` | Ch 28 | does not exist | Remove |
| `src/services/compact/cachedMicrocompact.ts` | Ch 28 | does not exist | Remove |
| `src/services/contextCollapse/` | Ch 28 | directory does not exist | Remove |
| `src/assistant/index.ts` | Ch 49 | does not exist | Remove |
| `src/assistant/gate.ts` | Ch 49 | does not exist | Remove |

#### 6.5.2 Files that EXIST at a DIFFERENT path (rename the entry)

| Plan path | Canonical path | Affected chapter(s) |
|---|---|---|
| `src/services/analytics/growthbook.js` | `src/services/analytics/growthbook.ts` | Ch 50 |
| `src/utils/cost-tracker.ts` | `src/cost-tracker.ts` | Ch 50 |

#### 6.5.3 Close-alternative replacements (add these to the chapter's source_files in addition to removing the missing ones)

| Chapter | Remove | Add instead |
|---|---|---|
| Ch 28 | `reactiveCompact.ts`, `cachedMicrocompact.ts` | `src/services/compact/apiMicrocompact.ts`, `src/services/compact/sessionMemoryCompact.ts`, `src/services/compact/compactWarningHook.ts` |
| Ch 49 | `index.ts`, `gate.ts` | (use only what exists: `sessionHistory.ts` + `src/buddy/` files as listed in the original brief) |

#### 6.5.4 Chapters whose remaining source files are sufficient after corrections

After applying 6.5.1–6.5.3, every chapter retains enough source files to be writable. No chapter is blocked. The step-0-runner does not escalate any of these as "needs_human_review" — they proceed to Step 1 normally.

#### 6.5.5 Logging requirements

The step-0-runner appends this event to `convergence.log` after applying corrections:

```json
{"ts": "<iso>", "event": "canonical_corrections_applied", "removed": [<list from 6.5.1>], "renamed": [<list from 6.5.2>], "added": [<list from 6.5.3>]}
```

---

## 7. Step-by-Step Workflow (hierarchical dispatch only — parent never executes step logic)

**Universal rule**: for every step below, the parent dispatches exactly ONE step-runner subagent per unit of work and reads ONLY the one-line STATUS response. The parent never does validation, file writes, file reads (except the allowed list in §5.5.2), or dispatches workers directly. If a step says "the parent does X," read that as "the parent dispatches a step-N-runner subagent that does X."

**Parent session startup sequence** (one time at the very start):
1. Invoke `superpowers:using-superpowers`
2. Invoke `superpowers:executing-plans`
3. Read `book-implementation-plan.md` Sections **1 through 7 only** via a single Read call with `offset=1, limit=1170`. Stop reading before Section 8 (which starts at ~line 1169). Sections 8.x template bodies are NOT read by the parent — ever.
4. Emit the 3-sentence confirmation (see prompt.md).
5. Proceed to Step 0 dispatch.

### Step 0 — Bootstrap (parent dispatches step-0-runner)

Parent action:
1. Dispatch ONE `step-0-runner` subagent via the Agent tool. The dispatch prompt is short:
   ```
   You are the step-0-runner. Read /home/hwzhang/build/open-claude-code/.book-build/templates/step-0-runner.md for your full instructions. If that file does not yet exist (this is the very first run), read book-implementation-plan.md Section 8.16.1 instead and follow those instructions verbatim. Execute Step 0 end-to-end. Your final response is exactly one line: STATUS: {"status":"done"|"failed","step":"0",...counts}.
   ```
2. Wait for STATUS response (expected ~5–15 minutes).
3. Read ONLY the STATUS line.
4. If `status=="failed"`, HALT and report to the user with the failure field. Do not proceed.
5. If `status=="done"`, read `.book-build/manifest.json` to confirm 57 chapter entries exist and `.book-build/repo-sha.txt` exists.
6. Proceed to Step 0.5.

Inside the step-0-runner, the subagent executes (per its template in §8.16.1):
- Create `.book-build/` and subdirectories
- Pin repo SHA
- Apply the canonical source file corrections from §6.5 to the chapter table — **no re-validation, no re-discovery**
- Pre-compute LOC hints for every file in the corrected chapter table and write `loc-hints/files.json`
- Seed `terminology.json` with 30 canonical terms
- Write `manifest.json` from the chapter table in §6.3 with §6.5 corrections applied
- Write `frontmatter.md` stub
- Append `bootstrap_done` and `canonical_corrections_applied` events to `convergence.log`

The step-0-runner does NOT extract HER excerpts, does NOT extract templates, and does NOT write `parent-state.md` — those are Step 0.5's job.

### Step 0.5 — Template extraction and parent-state snapshot (parent dispatches step-0.5-runner)

Parent action:
1. Dispatch ONE `step-0.5-runner` subagent. Dispatch prompt:
   ```
   You are the step-0.5-runner. Read book-implementation-plan.md Section 8.16.2 for your full instructions (on this very first run the template file does not yet exist on disk — you will create it). Execute Step 0.5 end-to-end. Your final response is exactly one line: STATUS: {"status":"done"|"failed","step":"0.5","worker_templates":<int>,"step_runner_templates":<int>,"her_excerpts":<int>,"parent_state_bytes":<int>}.
   ```
2. Wait for STATUS.
3. If `status=="failed"`, HALT and report.
4. If `status=="done"`, verify worker_templates=16, step_runner_templates=11, her_excerpts=57, parent_state_bytes is reasonable (4000–10000).
5. Proceed to Step 0.9.

Inside the step-0.5-runner, per its template in §8.16.2:
- Read Sections 8.1 through 8.15.1 of the plan ONE AT A TIME (via `offset`/`limit` on each) and write each to the corresponding file in `.book-build/templates/`. Also write the step-runner templates §8.16.1–§8.16.11 to `.book-build/templates/step-*.md`. Total: 27 template files (16 worker + 11 step-runner).
- Extract 57 HER excerpts to `.book-build/her-excerpts/chNN.md`. All 57 MUST be written. If any excerpt cannot be produced (e.g., the referenced HER section does not exist), write a placeholder note file and log it — do not skip.
- Write `.book-build/parent-state.md` with the 8 fields from §5.5.5 (~1000 words).
- Append `templates_extracted`, `her_excerpts_written` (with count 57), and `parent_state_written` events to `convergence.log`.

### Step 0.9 — Verification gate (parent dispatches step-0.9-runner; HARD GATE)

Parent action:
1. Dispatch ONE `step-0.9-runner` subagent. Dispatch prompt:
   ```
   You are the step-0.9-runner. Read .book-build/templates/step-0.9-runner.md for instructions. Run every check. Your final response is exactly one line: STATUS: {"status":"pass"|"fail","step":"0.9","checks_passed":<int>,"checks_failed":<int>,"failed_checks":[<names>]}.
   ```
2. Wait for STATUS.
3. **If `status=="pass"`**: proceed to Step 1.
4. **If `status=="fail"`**: HALT. Read the `failed_checks` list and report to the user. Do NOT proceed to Step 1 — the build is broken. The user decides whether to retry Step 0 and Step 0.5 or restart.

Inside the step-0.9-runner, per its template in §8.16.3, it runs these mandatory checks (all must pass):

1. `.book-build/manifest.json` exists AND contains exactly 57 chapter entries (`jq '.chapters | length'`)
2. Every chapter in manifest has a non-empty `source_files` array AND every listed path exists in the repo (after §6.5 corrections applied)
3. `.book-build/parent-state.md` exists AND word count ∈ [500, 1500]
4. `.book-build/templates/` contains all 27 expected files (16 worker + 11 step-runner)
5. Every template file is non-empty (min 200 bytes)
6. `.book-build/her-excerpts/` contains exactly 57 files (one per chapter)
7. Every HER excerpt file is non-empty (min 500 bytes, max 20000 bytes)
8. `.book-build/repo-sha.txt` exists and matches current `git rev-parse HEAD`
9. `.book-build/loc-hints/files.json` exists and is valid JSON
10. `.book-build/convergence.log` contains the events: `bootstrap_done`, `canonical_corrections_applied`, `templates_extracted`, `her_excerpts_written`, `parent_state_written`

The step-0.9-runner writes detailed pass/fail results to `.book-build/verification-0.9.md` and returns only the STATUS line.

### Step 1 — Chapter Writing (parent dispatches step-1-batch-runners; 15 batches: 14 × 4 + 1 × 1 chapters)

Parent action:
1. From `manifest.json`, identify the 14 batches: batch 1 = Ch 1–4, batch 2 = Ch 5–8, …, batch 14 = Ch 53–56, plus a final 1-chapter batch 15 = Ch 57. (Alternative: the runner handles the remainder.) Batches 1 (Part I) must complete before batch 15 (Part X last synthesis chapter) to respect the plan's Part-I-first ordering.
2. Dispatch 2 `step-1-batch-runner` subagents in parallel for the next 2 un-drafted batches. Dispatch prompt for each:
   ```
   You are the step-1-batch-runner for batch {{batch_num}} (chapters {{ch_range}}). Read .book-build/templates/step-1-batch-runner.md for instructions. Execute the batch end-to-end (dispatch 4 writer subagents in parallel, collect their STATUS lines, update manifest.json). Your final response is exactly one line: STATUS: {"status":"done"|"failed","step":"1","batch":{{batch_num}},"chapters_drafted":<int>,"chapters_failed":[<ids>]}.
   ```
3. Wait for both BATCH_DONE responses.
4. Read `manifest.json` to confirm the 8 chapters in those 2 batches are at `status="drafted"`.
5. Dispatch the next 2 batches.
6. Repeat until all 14 batches (or 15 with singleton) are done.
7. After every 4 BATCH_DONE responses, the parent re-reads `parent-state.md` to confirm its state is consistent. If anything looks off, HALT.

**Ordering constraint**: dispatch Part I (Ch 1–4) batch first and wait for its DONE before dispatching any non-Part-I batches. This ensures Part I terminology is set before later synthesis chapters get drafted.

**Parallelism**: at most 2 step-1-batch-runners in flight at any time. Each batch-runner internally dispatches 4 writer subagents in parallel, so the effective writer concurrency is 2×4=8. This stays within rate limits while keeping parent context flat.

Inside each step-1-batch-runner, per its template in §8.16.4:
- Read `parent-state.md` to confirm rules
- Read the 4 relevant chapter entries from `manifest.json`
- Dispatch 4 writer subagents in parallel (using the `.book-build/templates/chapter-writer.md` template)
- Each writer reads its own brief, its HER excerpt, its source files, and writes to `chapters/chNN-slug.md`
- The batch-runner reads ONLY each writer's STATUS line
- The batch-runner updates the 4 manifest entries to `status="drafted"` with counts
- The batch-runner appends 4 `writer_success` events to `convergence.log`
- The batch-runner returns ONE STATUS line for the whole batch

**Chapter failures inside a batch**: if a writer returns `status="failed"` or its counts are below minimums (citations < 6, diagrams < 2, snippets < 4, words < target × 0.85), the batch-runner marks that chapter `drafting_failed` in the manifest and returns with `chapters_failed: [<id>]` in its STATUS. The parent re-dispatches a fresh step-1-batch-runner for just the failed chapter(s) on the next iteration.

### Step 2 — Front Matter and Part Intros (parent dispatches step-2-runner)

Dispatched only after all Part I and Part II step-1-batch-runners have returned DONE.

Parent action: dispatch ONE `step-2-runner` subagent. Dispatch prompt:
```
You are the step-2-runner. Read .book-build/templates/step-2-runner.md for instructions. Execute Step 2 end-to-end. Your final response is exactly one line: STATUS: {"status":"done"|"failed","step":"2","frontmatter":"done"|"failed","part_intros":<int>}.
```

Wait for STATUS. If done, read manifest to confirm `frontmatter_status="drafted"` and all 10 part intros are drafted. Proceed to Step 3. If failed, HALT.

Inside the step-2-runner (§8.16.5):
- Dispatch 1 frontmatter writer subagent (`.book-build/templates/frontmatter-writer.md`) in parallel with 10 part-intro writer subagents (`.book-build/templates/part-intro-writer.md`)
- Collect their 11 STATUS lines
- Update manifest.json `frontmatter_status` and each part's `part_intro_status`
- Append 11 success events to convergence.log
- Return ONE STATUS line

### Step 3 — Per-Chapter Audit Fan-Out (parent dispatches step-3-chapter-runner per chapter, 2 in flight)

Parent action:
1. For each chapter with `status="drafted"` in manifest, dispatch a `step-3-chapter-runner` subagent. Dispatch prompt:
   ```
   You are the step-3-chapter-runner for chapter {{chapter_id}}. Read .book-build/templates/step-3-chapter-runner.md for instructions. Execute the 6-auditor fan-out for this chapter. Your final response is exactly one line: STATUS: {"status":"done"|"failed","step":"3","chapter":{{id}},"aggregate_verdict":"pass"|"revise"|"rewrite","verdicts":{<pass>:<verdict>,...}}.
   ```
2. Dispatch at most 2 step-3-chapter-runners in parallel (each internally runs 6 auditors, so effective auditor concurrency is 12 — matches the original cap).
3. Wait for STATUS. If `aggregate_verdict="pass"`, the chapter advances to `audited`. If `revise` or `rewrite`, record it for Step 4.
4. Continue dispatching until all chapters have reached an `audited` status or been queued for Step 4.

Inside the step-3-chapter-runner (§8.16.6):
- Dispatches 6 auditor subagents in parallel for the assigned chapter
- Reads their 6 verdict JSON files (which the auditors wrote)
- Computes the aggregate verdict (all pass → pass; any rewrite → rewrite; else revise)
- Updates the chapter's manifest entry
- Appends 6 audit events to convergence.log
- Returns ONE STATUS line

### Step 4 — Revision Dispatch (parent dispatches step-4-revision-runner per chapter that needs it)

For each chapter whose Step 3 aggregate verdict was `revise` or `rewrite`, the parent dispatches a `step-4-revision-runner`. Dispatch prompt:
```
You are the step-4-revision-runner for chapter {{chapter_id}}. Read .book-build/templates/step-4-revision-runner.md for instructions. Apply the appropriate revision (surgical reviser or fresh writer rewrite) based on the failed verdicts. Your final response is exactly one line: STATUS: {"status":"done"|"failed","step":"4","chapter":{{id}},"action":"revise"|"rewrite","pass_counts":{<updated>}}.
```

After DONE, the parent re-dispatches the step-3-chapter-runner for the same chapter (re-audit). Continue until the chapter reaches aggregate `pass` or hits a hard cap.

**Hard caps per chapter** (parent enforces by reading manifest's pass_counts before dispatching another revision):
- 5 passes per audit reason
- 3 total rewrites
- If any cap is exceeded, mark chapter `needs_critical_escalation` and defer to Step 5's critical reviewer. If the critical reviewer still can't rescue it after one more rewrite attempt, mark `needs_human_review` with a placeholder note.

Inside the step-4-revision-runner (§8.16.7):
- Reads the chapter's latest audit verdicts
- Decides revise vs rewrite based on the aggregate rule
- Dispatches either a reviser subagent (`.book-build/templates/reviser.md`) or a fresh writer subagent (`.book-build/templates/chapter-writer.md`) with the failure list
- Reads ONLY its STATUS line
- Updates manifest
- Returns ONE STATUS line

### Step 5 — Critical Reviewer Loop (parent dispatches step-5-critical-runner per round)

Parent action: per round (up to 4 rounds), dispatch ONE `step-5-critical-runner`. Dispatch prompt:
```
You are the step-5-critical-runner for round {{round}}. Read .book-build/templates/step-5-critical-runner.md for instructions. Execute the 3-reviewer parallel pass. Your final response is exactly one line: STATUS: {"status":"done","step":"5","round":{{round}},"hard_flags":<int>,"soft_flags":<int>,"chapters_to_revise":[<ids>]}.
```

After DONE, the parent reads `.book-build/critical-reviews/pass-{{round}}-merged.json` for the merged promotion list (the runner writes this). Re-dispatches step-4-revision-runners for flagged chapters. After Step 4 completes for those chapters, re-dispatches step-5-critical-runner for the next round.

Loop ends when two consecutive rounds produce zero `hard` flags, OR at 4 rounds (hard cap).

Inside the step-5-critical-runner (§8.16.8):
- Dispatches 3 critical reviewer subagents in parallel (`.book-build/templates/critical-reviewer.md`)
- Each reviewer samples different chapters and produces a flag list
- The runner merges the 3 outputs with the 2-of-3 hard promotion rule
- Writes the merged list to `.book-build/critical-reviews/pass-NN-merged.json`
- Appends a critical_round event to convergence.log
- Returns ONE STATUS line

### Step 6 — Global Stitch Audit (parent dispatches step-6-stitch-runner per round)

Parent action: dispatch ONE `step-6-stitch-runner`. Dispatch prompt:
```
You are the step-6-stitch-runner for round {{round}}. Read .book-build/templates/step-6-stitch-runner.md for instructions. Execute the single stitch audit. Your final response is exactly one line: STATUS: {"status":"done","step":"6","round":{{round}},"verdict":"pass"|"revise","issues":<int>}.
```

If `verdict="pass"`, proceed to Step 7. If `verdict="revise"`, read `.book-build/stitch-verdict.json` for the issue list, re-dispatch step-4-revision-runners for the affected chapters, then re-dispatch step-6-stitch-runner. Up to 3 stitch rounds total.

Inside the step-6-stitch-runner (§8.16.9):
- Dispatches 1 stitch auditor subagent (`.book-build/templates/stitch-auditor.md`)
- The auditor reads first/last 200 lines of each chapter and writes `stitch-verdict.json`
- Returns ONE STATUS line

### Step 7 — Back Matter Generation (parent dispatches step-7-backmatter-runner)

Parent action: dispatch ONE `step-7-backmatter-runner`. Dispatch prompt:
```
You are the step-7-backmatter-runner. Read .book-build/templates/step-7-backmatter-runner.md for instructions. Execute the 3-back-matter fan-out. Your final response is exactly one line: STATUS: {"status":"done","step":"7","glossary":"done"|"failed","bibliography":"done"|"failed","concordance":"done"|"failed"}.
```

Inside the runner (§8.16.10):
- Dispatches 3 back-matter writer subagents in parallel
- Each output runs through one accuracy audit pass
- Returns ONE STATUS line

### Step 8 — Final Stitch and Sanity Pass (parent dispatches step-8-final-runner)

Parent action: dispatch ONE `step-8-final-runner`. Dispatch prompt:
```
You are the step-8-final-runner. Read .book-build/templates/step-8-final-runner.md for instructions. Assemble the final file and run the sanity checks. Your final response is exactly one line: STATUS: {"status":"done"|"needs_revisions","step":"8","words":<int>,"diagrams":<int>,"snippets":<int>,"citations":<int>,"forbidden_tokens":<int>,"issues":[<strings>]}.
```

After DONE:
- If `status="done"` AND all counts pass sanity (see 10.2) AND `forbidden_tokens==0` AND `issues==[]`, AND this is the second consecutive Step 8 DONE with zero revisions → declare the book **DONE**, emit final status.
- If `status="needs_revisions"`, read the issues list, re-dispatch step-4-revision-runners for affected chapters, then re-dispatch step-8-final-runner. Up to 3 Step 8 rounds.

Inside the step-8-final-runner (§8.16.11), the runner performs:

1. Concatenate, in this exact order, into a single in-memory string:
   ```
   frontmatter.md
   parts/part-01-intro.md
   chapters/ch01-*.md … chapters/ch04-*.md (Part I)
   parts/part-02-intro.md
   chapters/ch05-*.md … chapters/ch10-*.md (Part II)
   parts/part-03-intro.md
   chapters/ch11-*.md … chapters/ch17-*.md (Part III)
   parts/part-04-intro.md
   chapters/ch18-*.md … chapters/ch24-*.md (Part IV)
   parts/part-05-intro.md
   chapters/ch25-*.md … chapters/ch31-*.md (Part V)
   parts/part-06-intro.md
   chapters/ch32-*.md … chapters/ch37-*.md (Part VI)
   parts/part-07-intro.md
   chapters/ch38-*.md … chapters/ch44-*.md (Part VII)
   parts/part-08-intro.md
   chapters/ch45-*.md … chapters/ch49-*.md (Part VIII)
   parts/part-09-intro.md
   chapters/ch50-*.md … chapters/ch52-*.md (Part IX)
   parts/part-10-intro.md
   chapters/ch53-*.md … chapters/ch57-*.md (Part X)
   back/glossary.md
   back/bibliography.md
   back/concordance.md
   ```
2. Write the concatenated string to `/home/hwzhang/build/open-claude-code/deep-research-of-cc-source-code.md` in a single Write-tool call.
3. **Run the final sanity pass** (parent only, no subagents):
   - Count total words. Must be ∈ [200,000, 310,000].
   - Count mermaid code blocks. Must be ≥ 114.
   - Regex-count citations matching `src/[^\s:]+\.tsx?(:L\d+(-L\d+)?)?`. Must be ≥ 342.
   - Grep for every forbidden token from Section 6.1. Any match → dispatch targeted reviser to the offending chapter (back to Step 4), then re-stitch (back to Step 8).
   - Grep for `[NEEDS-VERIFY]`. Any match → same.
   - Grep for `TODO`, `TBD`, `XXX`. Any match → same.
   - Verify every `src/path/file.ts` mentioned in a citation exists in the repo (random sample: 20 citations → ensure all exist). Any miss → targeted accuracy re-audit of the chapter.
4. **Convergence check**: if this is the second consecutive Step 8 run that produced no revisions (i.e., the previous Step 8 also passed sanity without any Step 4 dispatches), declare **DONE** and emit the final status. Otherwise, loop back to Step 3 for any chapter that had changes.
5. **Hard cap**: total of 3 full Step 8 stitch-and-sanity rounds before escape hatch.

### Step 9 — Escape Hatch (only if convergence fails)
1. Write `.book-build/convergence-failures.md` listing every remaining issue by chapter.
2. Emit a user-facing status: "Book not converged after 10 audit rounds and 3 stitch rounds. See `.book-build/convergence-failures.md`. Partial output written to `deep-research-of-cc-source-code.md`."
3. Halt.

---

## 8. Subagent Prompt Templates

These are the exact prompt templates GLM 5.1 must use when dispatching subagents. Variables in `{{double braces}}` are filled by the parent from `manifest.json` and other build-state files. **Do not paraphrase.** Every prompt ends with a mandatory machine-readable status line.

**Note on dispatch**: After Step 0.5, the parent does NOT embed the template bodies below inline in dispatch prompts. It either (a) passes the template file path and instructs the subagent to read it, or (b) reads the template file itself and composes a dispatch prompt from it, discarding the template from its own context immediately after dispatch. This is mandatory for parent context hygiene (see §5.5.1). Sections 8.1–8.15 below remain the canonical source of the template text, but they live on disk in `.book-build/templates/*.md` after Step 0.5.

### 8.0 Universal rules for every subagent (appended silently to every template)

The following rules apply to every template in §8.1–8.15 and must be included verbatim in every file under `.book-build/templates/` after Step 0.5. The parent enforces these structurally; subagents that violate them are treated as failures.

1. **Your final response is exactly one line.** That line is the `STATUS: {...}` JSON. No prose, no narration, no tool-call echoes, no explanations, no markdown headings, nothing before or after. The parent receives this line as your sole output and any extra bytes bloat its context.
2. **Do all your work via file writes and intermediate tool calls.** If you want to explain what you did, write it to a file under `.book-build/` (e.g., the audit prose reports go to `.md` files, chapter bodies go to chapter files). Do NOT put explanations in your final response.
3. **Do not restate the plan or the brief back to the user.** The parent already knows. Start working immediately.
4. **Do not invoke skills that will dump prose into your output.** Skills like `verification-before-completion` are allowed if they help you verify your work, but keep any skill-generated prose out of your final response.
5. **If you fail, still emit one line.** On failure, your final response is a STATUS line with `"status":"failed"` and a short `"error"` field. Do not return multi-line error explanations.
6. **You are forbidden from reading the plan file itself** (`book-implementation-plan.md`). Read only the template file, your brief, and the input files your template lists. **Exception**: step-0-runner and step-0.5-runner are exempt from this rule — they must read the plan file to bootstrap the build. After Step 0.5 completes, no subagent reads the plan.

### 8.1 Writer template

```
You are writing Chapter {{id}} of a 57-chapter technical book titled "Deep Research and Development Guide of CC Source Code". You are a single-use subagent. Your only job is to write this one chapter, then exit with a status line.

# Chapter brief
Title: {{title}}
Slug: {{slug}}
Target word count: {{target_words}} (acceptable range: {{min_words}}–{{max_words}})
Output file: /home/hwzhang/build/open-claude-code/.book-build/chapters/ch{{id_padded}}-{{slug}}.md

# Synopsis
{{synopsis}}

# Source files you MUST read and cite
{{source_files_list_with_loc_hints}}

Read each listed source file. For files > 1000 LOC, use offset/limit to target the relevant sections; the brief below names the key functions. For files < 1000 LOC read the full file. You are forbidden from citing files not on this list.

# HER (Harness Engineering Report) cross-references you MUST integrate
Read the pre-extracted excerpts at: /home/hwzhang/build/open-claude-code/.book-build/her-excerpts/ch{{id_padded}}.md
Cross-reference these exact HER sections: {{her_refs}}

# Diagram requirements (at minimum)
{{diagram_reqs_list}}
Only use: flowchart, sequenceDiagram, stateDiagram-v2, classDiagram, erDiagram. Each mermaid block goes in a fenced ```mermaid code block. No other diagram types.

# Terminology registry (authoritative)
Read /home/hwzhang/build/open-claude-code/.book-build/terminology.json and use the terms with the exact definitions given. If you need a new term, pick one that doesn't conflict; a consistency auditor will reconcile.

# House style (non-negotiable)
- Write the chapter body as Markdown. Use `##` for top-level headings inside the chapter (the chapter title itself is an `#` heading on the first line).
- Cite every factual claim with `src/path/to/file.ts:Lnnn` (line-number-exact). For ranges use `src/path/to/file.ts:Lnnn-Lmmm`. Citations go inline in backticks.
- You MUST have at least 6 unique citations, at least 2 mermaid diagrams, and **at least 4 quoted code snippets** from the listed source files.
- You MUST include these sections in this exact order after the chapter title:
  1. ## Overview
  2. ## Data structures and contracts (MUST include at least 1 quoted code snippet showing a type, schema, or interface definition from the codebase)
  3. ## Control flow (MUST include at least 1 mermaid diagram AND at least 1 quoted code snippet showing a key function body)
  4. ## Edge cases and failure modes
  5. ## Where cc diverges from the published pattern
  6. ## Developer takeaways for building a long-running agent
- **Quoted code snippet format** (strict):
  - Use a fenced code block tagged with the correct language (`typescript`, `tsx`, `javascript`, `json`, `bash`, `yaml`).
  - The FIRST line inside the fence must be a comment with the exact source path and line range plus a short caption. Examples:
    ```
    // src/query.ts:L341-L389 — queryLoop async generator entry
    ```
    or for a JSON/YAML block:
    ```
    # src/schemas/hooks.ts:L12-L34 — HookCommandSchema
    ```
  - The code inside the fence must be **verbatim** from the file (preserve whitespace, identifiers, and indentation exactly as in the source). You may trim the middle of long functions with a `// ...` line, but the kept lines must be literal copies.
  - Each snippet is 8–30 lines typical; 60 lines is the absolute maximum (for type/schema definitions that cannot be meaningfully trimmed).
  - Immediately after each snippet, write 2–6 sentences of explanation that reference specific identifiers from the snippet (e.g., "The `deps.callModel(…)` call on line L352 is where the streaming response begins.").
  - Every snippet must come from a file listed in your chapter brief. Do not invent code or quote from outside the brief.
  - Verify each snippet by reading the file yourself before writing it. If the line range in your head doesn't match the file, re-read the file and correct the range.
- The "Developer takeaways" section must be 150–300 words and must be a prose paragraph, not bullets.
- Forbidden tokens (will cause automatic revision if present): TODO, TBD, XXX, [NEEDS-VERIFY], "similar to chapter", "as noted earlier", "we will discuss", "as we shall see", "simply", "obviously", "just" (as filler word), "in summary" as a phrase.
- Do not reference future chapters you cannot verify exist. You may say "see Chapter N on X" only if N ∈ [1, 57] and the topic matches the synopsis.
- When a claim cannot be verified from a listed source file, OMIT the claim. Do not write [NEEDS-VERIFY] placeholders.
- When in doubt between prose and code: show the code. The book's value is in making the source legible, not in paraphrasing it.

# Operational constraints
- Read files ONCE and budget your reads. You have limited tool calls.
- Write ONLY to the output file path above. Do not modify any other file.
- The last thing you output before exiting must be this status line, on its own line, no trailing text:
  STATUS: {"status":"done","words":<int>,"citations":<int>,"diagrams":<int>,"snippets":<int>,"needs_verify":<int>,"brief_checksum":"{{brief_checksum}}"}

# Begin writing now.
```

### 8.2 Frontmatter writer template

```
You are a single-use subagent. Write the front matter file for the 57-chapter book "Deep Research and Development Guide of CC Source Code".

Output file: /home/hwzhang/build/open-claude-code/.book-build/frontmatter.md

Read:
- /home/hwzhang/build/open-claude-code/.book-build/manifest.json (full)
- First 20 lines only of each chapter file under .book-build/chapters/ (head, via offset/limit)

Produce (Markdown):
# Deep Research and Development Guide of CC Source Code
A byline line with the repo SHA from .book-build/repo-sha.txt.

## Preface
800–1,200 words. Explain the origin of the book (leaked cc codebase + HER report), why it exists, the intended audience (engineers building long-running agent harnesses), and the reading paths available (linear vs subsystem-focused vs pattern-focused).

## How to Read This Book
Explain citation format (src/path/file.ts:Lnnn), diagram conventions (the 5 allowed mermaid types), chapter structure (the 6 mandatory sections), and cross-reference notation.

## Table of Contents
Autogenerate from manifest.json. Structure:
- **Part I. Foundations and Framing**
  - Chapter 1. Why This Book Exists: The Harness Engineering Moment
  - ...
- **Part II. Entry, Bootstrap, and the Runtime Spine**
  - ...
Include all 57 chapters, 10 parts, 3 appendices.

## Forbidden Tokens
None in your output.

Status line (last line):
STATUS: {"status":"done","words":<int>}
```

### 8.3 Part intro writer template

```
You are a single-use subagent. Write the part intro for Part {{part_number}} ({{part_title}}) of the book.

Output file: /home/hwzhang/build/open-claude-code/.book-build/parts/part-{{part_padded}}-intro.md

Read ONLY:
- /home/hwzhang/build/open-claude-code/.book-build/manifest.json
- The `title` and `synopsis` fields of chapters belonging to Part {{part_number}} (do NOT read chapter bodies)

Write 500–900 words that:
1. State the theme of this Part
2. Explain why these chapters are grouped together
3. Preview each chapter in this Part in one sentence (listing chapter numbers and titles)
4. State what the reader should take away by the end of this Part
5. End with a back-pointer to the TOC in the front matter

Forbidden tokens: same as Writer template.

Status line:
STATUS: {"status":"done","words":<int>}
```

### 8.4 Accuracy Auditor template

```
You are a single-use audit subagent. You verify that chapter {{id}} accurately cites the cc codebase.

Read:
- The chapter file: /home/hwzhang/build/open-claude-code/.book-build/chapters/ch{{id_padded}}-{{slug}}.md
- Every source file listed in the brief for this chapter: {{source_files_list}}

Your job:
1. For every `src/path/file.ts:Lnnn` citation in the chapter, verify that (a) the file exists, (b) the line number is valid, (c) the line actually supports the surrounding claim.
2. For every factual claim NOT cited, verify it against the listed source files. If the claim is wrong or unsupported, flag it.
3. For every file in the listed source files that the chapter did NOT cite, note it as `uncited_source`. This is informational, not a fail.
4. **Verify every quoted code snippet**: for each fenced code block whose first line is a source-path comment like `// src/path.ts:L10-L30 — caption`, read the file at that exact range and compare character-by-character with the quoted block (ignoring the caption comment itself and any `// ...` trim markers). A snippet counts as:
   - `verbatim` if the non-trimmed lines match exactly (whitespace and identifiers preserved)
   - `drift` if identifiers match but whitespace differs
   - `hallucinated` if any non-trimmed line does not appear in the referenced range of the file
5. Count the total number of snippets. Minimum required: 4 per chapter.

Write your prose report to: /home/hwzhang/build/open-claude-code/.book-build/audits/ch{{id_padded}}/accuracy.md
Write your machine verdict to: /home/hwzhang/build/open-claude-code/.book-build/audits/ch{{id_padded}}/accuracy.verdict.json with schema:

{
  "verdict": "pass" | "revise" | "rewrite",
  "issues": [
    { "type": "bad_citation"|"unsupported_claim"|"wrong_file"|"wrong_line"|"snippet_hallucinated"|"snippet_drift"|"snippet_missing_caption",
      "chapter_line": <int>,
      "citation": "<string>",
      "problem": "<string>" }
  ],
  "uncited_sources": ["<path>"],
  "citation_total": <int>,
  "citation_verified": <int>,
  "snippet_total": <int>,
  "snippet_verbatim": <int>,
  "snippet_drift": <int>,
  "snippet_hallucinated": <int>
}

Verdict rules:
- "rewrite" if > 30% of citations are bad, OR ≥ 5 unsupported claims, OR any `snippet_hallucinated` > 0, OR `snippet_total` < 4
- "revise" if 1 ≤ problems ≤ those thresholds OR any `snippet_drift` > 0
- "pass" if zero issues AND `snippet_total` ≥ 4 AND every snippet is `verbatim`

Last line:
STATUS: {"status":"done","verdict":"<verdict>","issues":<count>,"snippets":<int>}
```

### 8.5 Cross-ref Auditor template

```
You are a single-use audit subagent. You verify that chapter {{id}} correctly cites and engages with the Harness Engineering Report (HER).

Read:
- Chapter: /home/hwzhang/build/open-claude-code/.book-build/chapters/ch{{id_padded}}-{{slug}}.md
- Required HER refs for this chapter: {{her_refs}}
- Pre-extracted HER excerpt: /home/hwzhang/build/open-claude-code/.book-build/her-excerpts/ch{{id_padded}}.md
- Full HER report at /home/hwzhang/.vim/claude/harness/harness-engineering-report.md (read only the relevant sections via offset/limit search for section headings)

Your job:
1. Verify the chapter cites ALL required HER refs listed in its brief. Missing any → revise or rewrite.
2. Verify each HER cite corresponds to a real section heading in HER.
3. Verify the chapter INCLUDES the mandatory "Where cc diverges from the published pattern" section and that it is substantive (not a one-liner).
4. Verify at least 2 distinct HER references are present.

Output report: audits/ch{{id_padded}}/cross-ref.md
Verdict JSON: audits/ch{{id_padded}}/cross-ref.verdict.json with schema:

{
  "verdict": "pass" | "revise" | "rewrite",
  "required_refs_found": <int>,
  "required_refs_total": <int>,
  "required_refs_missing": ["<her_ref_id>"],
  "divergence_section_length_words": <int>,
  "issues": [ { "type": "missing_ref"|"wrong_section"|"weak_divergence", "detail": "<string>" } ]
}

Verdict rules:
- "rewrite" if divergence section < 100 words OR missing ≥ half the required refs
- "revise" if 1 ≤ issues < those thresholds
- "pass" if all required refs present AND divergence section ≥ 150 words AND no bad refs

STATUS: {"status":"done","verdict":"<verdict>","issues":<count>}
```

### 8.6 Diagrams Auditor template

```
You are a single-use audit subagent. You verify that chapter {{id}}'s mermaid diagrams are syntactically valid and useful.

Read:
- Chapter: /home/hwzhang/build/open-claude-code/.book-build/chapters/ch{{id_padded}}-{{slug}}.md

Your job:
1. Extract every fenced ```mermaid … ``` block.
2. For each block, check:
   - The first non-empty line is one of: `flowchart`, `sequenceDiagram`, `stateDiagram-v2`, `classDiagram`, `erDiagram`. Anything else → invalid.
   - Balanced brackets: `[` `]`, `{` `}`, `(` `)`. Unbalanced → invalid.
   - Valid edge operators for the diagram type: flowchart uses `-->`, `---`, `-.->`, etc.; sequenceDiagram uses `->>`, `-->>`, `->`, `-->`; classDiagram uses `<|--`, `<|..`, `o--`, `*--`, `-->`. Unknown operators → invalid.
   - No Unicode arrows or smart quotes inside the diagram body.
   - At least 3 nodes/actors (otherwise diagram is "trivial" → useful=false).
3. Verify the diagram count is ≥ 2 (minimum) and ≥ {{required_diagrams}} (brief's requirement).

Output report: audits/ch{{id_padded}}/diagrams.md
Verdict JSON: audits/ch{{id_padded}}/diagrams.verdict.json with schema:

{
  "verdict": "pass" | "revise" | "rewrite",
  "diagram_count": <int>,
  "invalid_indices": [<int>],
  "trivial_indices": [<int>],
  "type_breakdown": { "flowchart": <int>, "sequenceDiagram": <int>, "stateDiagram-v2": <int>, "classDiagram": <int>, "erDiagram": <int> }
}

Verdict rules:
- "rewrite" if diagram_count < 2 OR > 50% invalid
- "revise" if any invalid OR any trivial (but count ≥ 2)
- "pass" otherwise

STATUS: {"status":"done","verdict":"<verdict>","count":<int>}
```

### 8.7 Polish Auditor template

```
You are a single-use audit subagent. You check prose quality, style, forbidden words, and word count for chapter {{id}}.

Read:
- Chapter: /home/hwzhang/build/open-claude-code/.book-build/chapters/ch{{id_padded}}-{{slug}}.md

Your job:
1. Count the total word count of the body (excluding mermaid code blocks and fenced code snippets).
2. Verify word_count ∈ [{{min_words}}, {{max_words}}].
3. Grep for forbidden tokens: TODO, TBD, XXX, [NEEDS-VERIFY], "similar to chapter", "as noted earlier", "we will discuss", "as we shall see", "simply" (standalone), "obviously", "just" (filler), "in summary" (as phrase).
4. Check the 6 mandatory sections are present in correct order.
5. Check the Developer takeaways section is a prose paragraph of 150–300 words.
6. Flag run-on sentences (> 60 words), passive-voice overload (> 40% passive), and unexplained jargon.
7. Count the fenced code snippets (blocks whose first line is a source-path caption comment like `// src/...`). Verify count ≥ 4.
8. Verify the "Data structures and contracts" section contains ≥ 1 code snippet.
9. Verify the "Control flow" section contains ≥ 1 code snippet.
10. Verify every snippet is 8–60 lines long (flag oversized blocks).
11. Verify every snippet is followed by 2–6 sentences of explanation (not just "The following code does X." one-liner).

Output report: audits/ch{{id_padded}}/polish.md
Verdict JSON: audits/ch{{id_padded}}/polish.verdict.json with schema:

{
  "verdict": "pass" | "revise" | "rewrite",
  "word_count": <int>,
  "forbidden_tokens_found": [{"token": "<string>", "count": <int>}],
  "sections_present": {
    "Overview": true|false,
    "Data structures and contracts": true|false,
    "Control flow": true|false,
    "Edge cases and failure modes": true|false,
    "Where cc diverges from the published pattern": true|false,
    "Developer takeaways for building a long-running agent": true|false
  },
  "takeaways_word_count": <int>,
  "snippet_count": <int>,
  "snippets_in_data_structures_section": <int>,
  "snippets_in_control_flow_section": <int>,
  "oversized_snippets": [{"index": <int>, "lines": <int>}],
  "unexplained_snippets": [<int>],
  "style_issues": [{"line": <int>, "issue": "<string>"}]
}

Verdict rules:
- "rewrite" if any required section missing OR word_count < min OR > 25% of sentences are run-on OR ≥ 5 forbidden tokens OR snippet_count < 4 OR snippets_in_data_structures_section < 1 OR snippets_in_control_flow_section < 1
- "revise" if word_count > max (> 25% over) OR 1 ≤ forbidden tokens < 5 OR takeaways out of range OR any oversized_snippets OR any unexplained_snippets
- "pass" otherwise

STATUS: {"status":"done","verdict":"<verdict>","words":<int>,"forbidden":<int>,"snippets":<int>}
```

### 8.8 Consistency Auditor template

```
You are a single-use audit subagent. You check terminology consistency for chapter {{id}}.

Read:
- Chapter: /home/hwzhang/build/open-claude-code/.book-build/chapters/ch{{id_padded}}-{{slug}}.md
- Terminology registry: /home/hwzhang/build/open-claude-code/.book-build/terminology.json
- Manifest: /home/hwzhang/build/open-claude-code/.book-build/manifest.json (for titles only, not bodies)

Your job:
1. For each term in terminology.json that appears in this chapter, verify it is used with the canonical definition.
2. Flag any alternate names for registered terms (e.g., "sub-agent" vs "subagent", "tool call" vs "tool use" when the registry says "tool use").
3. Detect voice drift: is the chapter written in the same tense and register as the house style ("present tense, descriptive, cite-heavy")?
4. Propose new terms that this chapter introduced and that should be added to the registry. Do NOT add them yourself; the parent will merge.

Output report: audits/ch{{id_padded}}/consistency.md
Verdict JSON: audits/ch{{id_padded}}/consistency.verdict.json:

{
  "verdict": "pass" | "revise" | "rewrite",
  "term_conflicts": [{"term": "<string>", "used_as": "<string>", "should_be": "<string>"}],
  "voice_drift": "<description>" | null,
  "proposed_new_terms": [{"term": "<string>", "definition": "<string>"}]
}

Verdict rules:
- "rewrite" if > 5 term conflicts OR severe voice drift
- "revise" if 1 ≤ term conflicts ≤ 5 OR mild voice drift
- "pass" if zero

STATUS: {"status":"done","verdict":"<verdict>","conflicts":<int>}
```

### 8.9 Gaps Auditor template

```
You are a single-use audit subagent. You ask: "what did the chapter fail to cover that its brief required?"

Read:
- Chapter: /home/hwzhang/build/open-claude-code/.book-build/chapters/ch{{id_padded}}-{{slug}}.md
- Chapter brief (from manifest): {{chapter_brief_json}}
- Directory listing of each folder referenced in source_files (to detect missing siblings the chapter should have mentioned)

Your job:
1. Verify the chapter's Overview matches the synopsis in the brief.
2. Verify every source file in the brief is cited at least once. Uncited files → flag as gap.
3. Verify every mandated diagram (by type and topic) exists.
4. Verify the chapter has the minimum citation count (6), diagram count (2), and snippet count (4) from the brief.
5. Verify that at least one snippet comes from each of the **top 3 source files** in the brief (the files most central to the chapter's topic). A chapter that cites Tool.ts but never shows actual Tool.ts code is a gap.
6. Identify topics a reasonable reader would expect but that are missing (e.g., in Ch 14 The Bash Tool, if sandboxing is in the brief but the chapter doesn't discuss sandbox detection, flag it).

Output report: audits/ch{{id_padded}}/gaps.md
Verdict JSON: audits/ch{{id_padded}}/gaps.verdict.json:

{
  "verdict": "pass" | "revise" | "rewrite",
  "uncited_brief_files": ["<path>"],
  "missing_diagrams": [{"required": "<topic>", "found": false}],
  "uncovered_topics": ["<string>"],
  "citation_count": <int>,
  "diagram_count": <int>,
  "snippet_count": <int>,
  "top_files_without_snippets": ["<path>"]
}

Verdict rules:
- "rewrite" if ≥ 3 uncited brief files OR any mandated diagram missing OR citation_count < 6 OR diagram_count < 2 OR snippet_count < 4 OR ≥ 2 top_files_without_snippets
- "revise" if 1–2 uncited brief files OR 1–3 uncovered_topics OR 1 top file without a snippet
- "pass" otherwise

STATUS: {"status":"done","verdict":"<verdict>","gaps":<int>,"snippets":<int>}
```

### 8.10 Reviser template

```
You are a single-use reviser subagent. You perform surgical edits to an existing chapter.

Read:
- The existing chapter: /home/hwzhang/build/open-claude-code/.book-build/chapters/ch{{id_padded}}-{{slug}}.md
- Failed verdicts: {{failed_verdict_paths}} (you read each one)
- Source files from the original brief (for any re-verification needed): {{source_files_list}}
- HER excerpt: /home/hwzhang/build/open-claude-code/.book-build/her-excerpts/ch{{id_padded}}.md

Your job is to fix ONLY the issues listed in the failed verdicts. Do NOT rewrite the whole chapter. Do NOT remove any working content. For each flagged issue:
- bad_citation → verify against the source file and correct the line number, or remove the claim if unverifiable.
- missing_ref → add a paragraph that cross-references the required HER section.
- missing_diagram → add the diagram with the correct type.
- forbidden_token → rewrite the offending sentence to remove the token.
- term_conflict → replace the non-canonical form with the canonical one.
- missing_section → add the section with at least 300 words of substantive content.
- uncited_brief_file → add a paragraph citing the file.
- weak_divergence → extend the divergence section to ≥ 200 words with concrete examples.

Output: overwrite the chapter file in place. Preserve every section that was not flagged.

Status line:
STATUS: {"status":"done","issues_addressed":<int>,"words":<int>}
```

### 8.11 Critical Reviewer template

```
You are a single-use critical reviewer subagent. You are NOT an auditor; your job is to find weak, shallow, or disconnected chapters.

Read:
- /home/hwzhang/build/open-claude-code/.book-build/manifest.json
- Your assigned random sample of 3 chapters per part × 10 parts = 30 chapter files (list: {{sampled_chapter_paths}})
- /home/hwzhang/.vim/claude/harness/harness-engineering-report.md (read only the sections cross-referenced by the sampled chapters)

Your job:
1. For each sampled chapter, read it end to end. Ask:
   - Is this chapter DEEP? Does it go beyond surface-level description into actual mechanics?
   - Are the diagrams useful or decorative?
   - Is the HER cross-reference substantive or token?
   - Does the "Where cc diverges from the published pattern" section say something real?
   - Does the chapter connect to other chapters in a way a thoughtful reader would expect?
   - Is there a claim or mechanism this chapter SHOULD have mentioned but did not?
2. Be skeptical. Your goal is to find chapters that "tick the boxes" without actually teaching the reader.
3. Produce a prioritized list. Use `hard` severity when the chapter fundamentally fails its purpose; use `soft` when it ticks the boxes but is weak.

Output file: /home/hwzhang/build/open-claude-code/.book-build/critical-reviews/pass-{{pass_number}}-reviewer-{{reviewer_number}}.json with schema:

{
  "reviewer": {{reviewer_number}},
  "pass": {{pass_number}},
  "flags": [
    {
      "chapter_id": <int>,
      "severity": "hard" | "soft",
      "reasons": ["<string>"],
      "suggested_angle": "<string, 1-2 sentences>"
    }
  ]
}

A chapter with NO flags is considered acceptable by this reviewer. It is expected that most chapters will receive no flags. Prefer quality of criticism over quantity.

Status line:
STATUS: {"status":"done","flags":<int>,"hard_flags":<int>}
```

### 8.12 Stitch Auditor template

```
You are a single-use stitch auditor subagent. You verify the book holds together.

Read:
- /home/hwzhang/build/open-claude-code/.book-build/manifest.json (full)
- /home/hwzhang/build/open-claude-code/.book-build/terminology.json
- /home/hwzhang/build/open-claude-code/.book-build/frontmatter.md (full)
- First 200 AND last 200 lines of every chapter file (via offset/limit)
- First 100 lines of every part intro

Your job (check each, report issues):
1. **TOC match**: frontmatter.md TOC entries correspond 1:1 with manifest.json chapters and part intros.
2. **Monotone numbering**: chapter files are named ch01…ch57, chapters in order, no gaps.
3. **No duplicate top-level headings**: every chapter's H1 is unique; no two chapters have the same `# Title`.
4. **Forward references**: every `Chapter N` mention in the read slice (head + tail) refers to an N ∈ [1, 57] that exists in the manifest. A mention of a chapter that does not exist → flag.
5. **Terminology vs registry**: any term in a chapter head/tail that conflicts with terminology.json → flag.
6. **Part intro back-pointer**: every part intro's last 10 lines mention "the Table of Contents" or equivalent.
7. **No orphan placeholders**: head/tail of no chapter contains forbidden tokens.
8. **No double-chapter content**: no two chapters have identical first 5 lines (header duplicates).

Output prose report: (no prose output; write directly to verdict JSON)
Verdict JSON: /home/hwzhang/build/open-claude-code/.book-build/stitch-verdict.json:

{
  "verdict": "pass" | "revise",
  "issues": [{"type": "<string>", "chapter_id": <int|null>, "detail": "<string>"}]
}

Verdict rules:
- "pass" if zero issues
- "revise" otherwise

Status line:
STATUS: {"status":"done","verdict":"<verdict>","issues":<int>}
```

### 8.13 Glossary back-matter writer template

```
You are a single-use back-matter subagent. Write the glossary.

Read:
- /home/hwzhang/build/open-claude-code/.book-build/terminology.json
- The "Key Terms and Taxonomy" content of /home/hwzhang/.vim/claude/harness/harness-engineering-report.md (sections 1, 2, 4 headers)

Output file: /home/hwzhang/build/open-claude-code/.book-build/back/glossary.md

Produce:
# Glossary

Alphabetized list of terms. For each:
- **term** — definition (2-3 sentences). If the term came from HER, tag with `(HER §N)`. If from cc, tag with the subsystem folder.

Target: every term from terminology.json plus ~20 HER terms. Approximately 2000–2500 words.

Forbidden tokens: same as usual.

STATUS: {"status":"done","terms":<int>}
```

### 8.14 Bibliography back-matter writer template

```
You are a single-use back-matter subagent. Write the bibliography.

Read:
- §22 Sources of /home/hwzhang/.vim/claude/harness/harness-engineering-report.md (all 60+ entries)
- /home/hwzhang/build/open-claude-code/README.md
- Any top-level docs in /home/hwzhang/build/open-claude-code/docs/ or equivalent (list first, then read)

Output file: /home/hwzhang/build/open-claude-code/.book-build/back/bibliography.md

Produce:
# Bibliography

Numbered list (matching HER where possible), grouped by section:
## Canonical Sources
## Taxonomy and Theory
## Practitioner Perspectives
## Academic Papers
## Platform Documentation
## cc Internal References

Each entry: author, title, URL (if available), year. Approximately 1500–2500 words.

STATUS: {"status":"done","entries":<int>}
```

### 8.15 Concordance back-matter writer template

```
You are a single-use back-matter subagent. Write the source-file concordance.

Read:
- /home/hwzhang/build/open-claude-code/.book-build/manifest.json (full; you need every chapter's source_files list)

Output file: /home/hwzhang/build/open-claude-code/.book-build/back/concordance.md

Produce:
# Source File Concordance

Alphabetized list of every `src/...` path that appears in any chapter's source_files. For each path:
- **src/path/to/file.ts** — Chapters 7, 12, 22 (comma-separated numbers)

Include a summary section at the bottom: total unique files cited, total chapters-with-citations, top 10 most-cited files.

Target: every file that appears in any chapter brief. Approximately 2000–4000 words.

STATUS: {"status":"done","files":<int>}
```

### 8.15.1 Back-matter auditor template

```
You are a single-use back-matter audit subagent. You verify the quality and completeness of a back-matter file.

Read:
- The back-matter file: {{back_matter_path}} (one of: glossary.md, bibliography.md, or concordance.md)
- /home/hwzhang/build/open-claude-code/.book-build/terminology.json (for glossary audits)
- /home/hwzhang/build/open-claude-code/.book-build/manifest.json (for concordance audits)

Your job depends on the file type:

**For glossary.md:**
1. Every term in terminology.json appears in the glossary.
2. Definitions are 2–3 sentences, not one-liners.
3. Terms are alphabetized.
4. No forbidden tokens.

**For bibliography.md:**
1. Entries are grouped by section (Canonical Sources, Taxonomy and Theory, etc.).
2. Each entry has author, title, and year at minimum.
3. At least 50 entries total (HER cites 60+ sources).
4. No forbidden tokens.

**For concordance.md:**
1. Every file path in any chapter's source_files (from manifest) appears in the concordance.
2. Chapter numbers listed for each file are plausible (i.e., that chapter lists the file in its brief).
3. The summary section exists with total counts and top-10 list.
4. No forbidden tokens.

Write your verdict to: /home/hwzhang/build/open-claude-code/.book-build/audits/back-matter/{{file_type}}.verdict.json with schema:

{
  "verdict": "pass" | "revise",
  "issues": [{"type": "<string>", "detail": "<string>"}]
}

Verdict rules:
- "revise" if any issue found
- "pass" if zero issues

STATUS: {"status":"done","verdict":"<verdict>","issues":<count>}
```

### 8.16 Step-runner templates

Step-runners are a special class of subagent. They execute one phase of the workflow, internally dispatching worker subagents, and return a single STATUS line. The parent dispatches only step-runners, never workers directly. All step-runner templates below must be extracted to `.book-build/templates/step-*.md` by the step-0.5-runner at Step 0.5 (per §5.5.4).

### 8.16.1 step-0-runner template

```
You are the step-0-runner. Your sole job is to execute Step 0 (bootstrap) of the book build.

Inputs:
- /home/hwzhang/build/open-claude-code/book-implementation-plan.md (read only §6.3 chapter table and §6.5 corrections table)
- The repo at /home/hwzhang/build/open-claude-code/

Outputs (you MUST create all of these):
- /home/hwzhang/build/open-claude-code/.book-build/ directory and subdirectories (chapters, her-excerpts, loc-hints, audits, critical-reviews, parts, back, templates)
- For each chapter 1-57, the directory .book-build/audits/chNN/
- .book-build/repo-sha.txt — output of `git -C /home/hwzhang/build/open-claude-code rev-parse HEAD`
- .book-build/terminology.json — JSON object with "version":1 and "terms" array seeded with the 30 canonical terms from §5.5.5 (tool, subagent, fork, worktree, task, skill, slash command, hook, permission mode, classifier, compaction, microcompact, context collapse, autocompact, memory, memdir, session, plan mode, harness, sensor, guide, back-pressure, deferred tool, ToolSearch, PreToolUse, PostToolUse, manifest, generator-evaluator, checkpoint-restore, observation masking). For each term include a one-sentence canonical definition you write yourself based on the plan.
- .book-build/loc-hints/files.json — JSON object mapping every source file path that appears in §6.3 (after §6.5 corrections) to its line count (run `wc -l` on each). Only include files that exist on disk after §6.5 corrections.
- .book-build/manifest.json — JSON object built from the §6.3 chapter table, with §6.5 corrections applied (remove files in §6.5.1, rename per §6.5.2, add per §6.5.3). Each chapter entry has: id, id_padded, title, slug, part, synopsis, source_files (corrected), her_refs, diagram_reqs, target_words, min_words (target*0.85), max_words (target*1.25), status="pending", pass_counts={accuracy:0,crossref:0,diagrams:0,polish:0,consistency:0,gaps:0,critical:0}, last_verdict=null, word_count=null, citations=null, diagram_count=null, snippet_count=null, snippets_verbatim=null, snippets_drift=null, snippets_hallucinated=null, needs_verify=null, brief_checksum="". Also include a parts array with 10 part objects.
  IMPORTANT source_files rules:
  (a) For Ch 2, 53, 54, 55, 56, 57: set source_files to [] — these are meta/synthesis chapters with no primary source reads per §6.3.
  (b) For all other chapters: if §6.3 lists a directory path (e.g., `src/tools/FileReadTool/`), expand it to the actual .ts/.tsx files inside that directory using `ls` or `find`. Never store directory paths in source_files — only regular file paths.
  (c) After applying §6.5.1 removals, verify every remaining source_files entry exists on disk with `test -f`. Log any unexpected missing files to unexpected-missing.json.
- .book-build/frontmatter.md — stub with just the title line and a "TBD" placeholder TOC (will be replaced at Step 2).
- .book-build/convergence.log — append (create if not exists) two JSONL events: {"ts":"<iso>","event":"bootstrap_done","chapter_count":57,"missing_files":[<from §6.5.1>],"renamed_files":[<from §6.5.2>]} and {"ts":"<iso>","event":"canonical_corrections_applied","removed":[...],"renamed":[...],"added":[...]}

RULES:
- Apply §6.5 corrections WITHOUT re-validation. The plan is authoritative for those specific files.
- For all OTHER files in the chapter table (not covered by §6.5), verify they exist. If any are missing, log them to a new .book-build/unexpected-missing.json file and append a convergence event, but do NOT halt.
- Do NOT read any source files beyond what `wc -l` needs (or use `wc -l` directly via Bash which doesn't put content in your context).
- Do NOT extract HER excerpts (that is Step 0.5's job).
- Do NOT write templates (that is Step 0.5's job).

Your final response is EXACTLY one line, no prose before or after:
STATUS: {"status":"done"|"failed","step":"0","chapters":57,"missing_files":<int>,"renamed_files":<int>,"loc_hints":<int>}
```

### 8.16.2 step-0.5-runner template

```
You are the step-0.5-runner. Your sole job is to execute Step 0.5 of the book build: extract all templates from the plan file, extract all HER excerpts, and write parent-state.md.

Inputs:
- /home/hwzhang/build/open-claude-code/book-implementation-plan.md (you will read §8.1 through §8.16 one at a time)
- /home/hwzhang/.vim/claude/harness/harness-engineering-report.md (read once to extract excerpts)
- /home/hwzhang/build/open-claude-code/.book-build/manifest.json (read once for chapter list and her_refs)

Outputs:
1. Worker template files (16 files in .book-build/templates/):
   - Read plan §8.1 via offset/limit; extract the fenced template block; write to .book-build/templates/chapter-writer.md
   - Read plan §8.2; write to .book-build/templates/frontmatter-writer.md
   - Read plan §8.3; write to .book-build/templates/part-intro-writer.md
   - Read plan §8.4; write to .book-build/templates/accuracy-auditor.md
   - Read plan §8.5; write to .book-build/templates/crossref-auditor.md
   - Read plan §8.6; write to .book-build/templates/diagrams-auditor.md
   - Read plan §8.7; write to .book-build/templates/polish-auditor.md
   - Read plan §8.8; write to .book-build/templates/consistency-auditor.md
   - Read plan §8.9; write to .book-build/templates/gaps-auditor.md
   - Read plan §8.10; write to .book-build/templates/reviser.md
   - Read plan §8.11; write to .book-build/templates/critical-reviewer.md
   - Read plan §8.12; write to .book-build/templates/stitch-auditor.md
   - Read plan §8.13; write to .book-build/templates/glossary-writer.md
   - Read plan §8.14; write to .book-build/templates/bibliography-writer.md
   - Read plan §8.15; write to .book-build/templates/concordance-writer.md
   - Read plan §8.15.1; write to .book-build/templates/back-matter-auditor.md

2. Step-runner template files (11 files in .book-build/templates/):
   - Read plan §8.16.1; write to .book-build/templates/step-0-runner.md
   - Read plan §8.16.2; write to .book-build/templates/step-0.5-runner.md (yes, even your own template)
   - Read plan §8.16.3; write to .book-build/templates/step-0.9-runner.md
   - Read plan §8.16.4; write to .book-build/templates/step-1-batch-runner.md
   - Read plan §8.16.5; write to .book-build/templates/step-2-runner.md
   - Read plan §8.16.6; write to .book-build/templates/step-3-chapter-runner.md
   - Read plan §8.16.7; write to .book-build/templates/step-4-revision-runner.md
   - Read plan §8.16.8; write to .book-build/templates/step-5-critical-runner.md
   - Read plan §8.16.9; write to .book-build/templates/step-6-stitch-runner.md
   - Read plan §8.16.10; write to .book-build/templates/step-7-backmatter-runner.md
   - Read plan §8.16.11; write to .book-build/templates/step-8-final-runner.md

3. HER excerpts (57 files in .book-build/her-excerpts/):
   - Read the full HER report ONCE (it is ~1300 lines; reading it once is acceptable here since you exit after this step)
   - For each chapter 1–57, read the chapter's her_refs from manifest.json, find those sections in HER, and write an excerpt file .book-build/her-excerpts/chNN.md containing only the referenced sections (max ~4000 words each)
   - If a her_ref is vague (e.g., "§6 all 17 failure modes"), include the entire referenced section
   - Every chapter MUST get an excerpt file, even if small. If a her_ref cannot be located, write a placeholder file noting which refs are missing and log a warning to convergence.log
   - Verify all 57 files exist before returning

4. Parent state snapshot (.book-build/parent-state.md):
   - ~1000 words, 8 sections matching §5.5.5:
     A. Current Step Tracker — "Step 0.5 complete. Step 1 pending. 0/57 drafted."
     B. Hard Caps — 5 passes per reason per chapter, 3 rewrites per chapter, 4 critical reviewer passes, 3 stitch rounds, 10 total audit rounds, 3 Step 8 rounds
     C. Forbidden Tokens — TODO, TBD, XXX, [NEEDS-VERIFY], "similar to chapter", "as noted earlier", "we will discuss", "as we shall see", "simply", "obviously", "just" (as filler), "in summary" (as phrase)
     D. Convergence Criteria — one-paragraph summary: every chapter done iff all 6 audits pass + counts in bounds + citations valid + snippets verbatim; book done iff all chapters done + stitch pass + back matter audited + two consecutive zero-revision sweeps
     E. Step-Runner Template Paths — the 11 step-runner file paths
     F. Worker Template Paths — the 16 worker file paths
     G. Key Directory Paths — manifest, convergence.log, chapters/, audits/, her-excerpts/, templates/, parent-state.md
     H. Parallelism Caps — 2 step-1-batch-runners in flight, 2 step-3-chapter-runners in flight, 3 critical runners (inside step-5-critical-runner), 1 stitch runner, 1 final runner

5. Convergence events (append to .book-build/convergence.log as JSONL):
   - {"ts":"<iso>","event":"templates_extracted","worker_count":16,"step_runner_count":11}
   - {"ts":"<iso>","event":"her_excerpts_written","count":57}
   - {"ts":"<iso>","event":"parent_state_written","bytes":<int>}

RULES:
- Read §8.1 through §8.16 one at a time using offset/limit, not in bulk. This keeps your context clean.
- Do NOT echo template content in your own response.
- Do NOT re-read the plan file after you finish the 27 template extractions.
- The HER report read is unavoidable — do it once, write all 57 excerpts immediately, then never re-read.
- You have at most 90 minutes wall clock to complete this step.
- **Context budget warning**: this step requires ~30 reads and ~85 writes. If your context climbs above 70%, prioritize completing templates first (they are needed by all downstream steps), then HER excerpts, then parent-state.md. If you cannot complete all work, return status="failed" with a count of what was completed so the parent can re-dispatch for the remainder.

Your final response is EXACTLY one line:
STATUS: {"status":"done"|"failed","step":"0.5","worker_templates":<int>,"step_runner_templates":<int>,"her_excerpts":<int>,"parent_state_bytes":<int>}
```

### 8.16.3 step-0.9-runner template

```
You are the step-0.9-runner. Your sole job is to verify that Step 0 and Step 0.5 produced a valid build state. This is a HARD GATE: if any check fails, return "fail" and let the parent halt.

Inputs: (read only these)
- .book-build/manifest.json
- .book-build/parent-state.md
- .book-build/repo-sha.txt
- .book-build/loc-hints/files.json
- .book-build/convergence.log (tail)
- Directory listings of .book-build/templates/, .book-build/her-excerpts/, .book-build/audits/

Checks (every check must pass; if any fails, aggregate verdict is "fail"):

C1. manifest.json exists and has exactly 57 chapters (jq '.chapters | length')
C2. Every chapter in manifest has a non-empty source_files array, EXCEPT Ch 2, 53, 54, 55, 56, 57 which are meta/synthesis chapters with intentionally empty source_files per §6.3 (mark these PASS automatically)
C3. Every file in every chapter's source_files exists as a regular file on disk (use `test -f` or `ls`; do NOT read contents). Note: if any entry is a directory path (trailing slash or `test -d` true), that is a FAIL — the step-0-runner should have expanded directories to their contained .ts/.tsx files
C4. parent-state.md exists, is readable, and word count ∈ [500, 1500]
C5. .book-build/templates/ contains all 27 files: the 16 worker templates and the 11 step-runner templates (names per §5.5.4)
C6. Every template file is non-empty (min 200 bytes)
C7. .book-build/her-excerpts/ contains exactly 57 files (ch01.md through ch57.md)
C8. Every HER excerpt file has size between 500 and 20000 bytes
C9. repo-sha.txt exists and its content matches current `git -C /home/hwzhang/build/open-claude-code rev-parse HEAD`
C10. loc-hints/files.json is valid JSON and contains at least 50 entries
C11. .book-build/audits/ contains 57 subdirectories (ch01/ through ch57/)
C12. convergence.log contains all these events: bootstrap_done, canonical_corrections_applied, templates_extracted, her_excerpts_written, parent_state_written

Write a detailed pass/fail report to .book-build/verification-0.9.md listing every check and its outcome.

Your final response is EXACTLY one line:
STATUS: {"status":"pass"|"fail","step":"0.9","checks_passed":<int>,"checks_failed":<int>,"failed_checks":[<names>]}
```

### 8.16.4 step-1-batch-runner template

```
You are the step-1-batch-runner for batch {{batch_num}} (chapters {{ch_range}}, typically 4 chapters).

Inputs:
- .book-build/parent-state.md (read first to get rules)
- .book-build/manifest.json (read to get the 4 chapter briefs)
- .book-build/templates/chapter-writer.md (read once; you will pass its path to workers)
- .book-build/her-excerpts/chNN.md for each chapter in the batch (DO NOT read these yourself; workers read them)
- .book-build/loc-hints/files.json (read once to see file sizes)

Procedure:
1. Read parent-state.md and manifest.json.
2. **Gate check**: verify .book-build/verification-0.9.md exists and contains "pass". If not, return STATUS with status="failed" and error="step_0.9_gate_not_passed". Do NOT proceed to writing.
3. For each chapter in the batch (up to 4 chapters), dispatch a writer subagent in parallel via the Agent tool. The writer's prompt is:
   "You are a single-use chapter writer subagent. Read .book-build/templates/chapter-writer.md for your full instructions. Your chapter is {{chapter_id}}. Read .book-build/manifest.json for your brief. Read .book-build/her-excerpts/ch{{id_padded}}.md for HER cross-references. Read .book-build/loc-hints/files.json for file size hints. Execute the writer template end-to-end. Write your output to .book-build/chapters/ch{{id_padded}}-{{slug}}.md. Your final response is exactly one line: STATUS: {...}."
4. Wait for all writer STATUS responses (they run in parallel).
5. For each writer response:
   - Parse the STATUS JSON
   - If status=="done" AND counts meet minimums (words ≥ target*0.85, citations ≥ 6, diagrams ≥ 2, snippets ≥ 4), update manifest.json for that chapter to status="drafted" with the counts
   - If status=="failed" OR counts below minimums, update manifest.json to status="drafting_failed" and include the chapter in your own batch STATUS under chapters_failed
6. Append writer_success or writer_failure events to convergence.log for each chapter
7. Update parent-state.md's "Current Step Tracker" field to reflect the new drafted count

RULES:
- Do NOT read any chapter body file. The writers write to disk; you just read their STATUS lines.
- Do NOT read the plan file, source files, or HER excerpts.
- Do NOT embed the chapter-writer template text in your worker dispatches — pass the path only.
- At most 4 writers in flight at once (you are handling one batch of 4).

Your final response is EXACTLY one line:
STATUS: {"status":"done"|"failed","step":"1","batch":{{batch_num}},"chapters_drafted":<int>,"chapters_failed":[<ids>]}
```

### 8.16.5 step-2-runner template

```
You are the step-2-runner. Your sole job is Step 2: front matter + 10 part intros.

Inputs:
- .book-build/parent-state.md
- .book-build/manifest.json
- .book-build/templates/frontmatter-writer.md
- .book-build/templates/part-intro-writer.md

Procedure:
1. Verify all Part I and Part II chapters are at status="drafted" or later in manifest. If not, return status="failed" with reason "prerequisite_not_met".
2. Dispatch 1 frontmatter writer subagent via Agent tool. Its prompt passes the template path .book-build/templates/frontmatter-writer.md and tells it to read that file.
3. In parallel, dispatch 10 part-intro writer subagents, one per part (1-10). Each writer's prompt tells it which part it's writing for.
4. Collect 11 STATUS lines.
5. Update manifest.json frontmatter_status and parts[N].intro_status for each part.
6. Append 11 success events to convergence.log.
7. Update parent-state.md current step tracker.

Your final response is EXACTLY one line:
STATUS: {"status":"done"|"failed","step":"2","frontmatter":"done"|"failed","part_intros_done":<int>}
```

### 8.16.6 step-3-chapter-runner template

```
You are the step-3-chapter-runner for chapter {{chapter_id}}. Your sole job is to fan out the 6 auditors for this chapter and compute the aggregate verdict.

Inputs:
- .book-build/parent-state.md
- .book-build/manifest.json (read just this chapter's entry)
- .book-build/templates/accuracy-auditor.md through gaps-auditor.md (6 files; you pass paths to workers, do not read contents yourself)

Procedure:
1. Dispatch 6 auditor subagents in parallel for this chapter. Each auditor's prompt passes the relevant template path.
2. Wait for 6 STATUS responses.
3. Each auditor also writes a .verdict.json file to .book-build/audits/chNN/<pass>.verdict.json.
4. Read the 6 verdict JSON files (tiny, acceptable).
5. Compute aggregate verdict:
   - If all 6 are "pass" → aggregate = "pass"
   - If any is "rewrite" → aggregate = "rewrite"
   - Else (one or more "revise", no "rewrite") → aggregate = "revise"
6. Update manifest.json for this chapter: pass_counts.<reason> +1 for each verdict, last_verdict=aggregate. If aggregate=="pass", status="audited".
7. If the consistency auditor's verdict JSON contains `proposed_new_terms`, merge up to 5 new terms into .book-build/terminology.json (read → append → write). Skip duplicates.
8. Append 6 audit events to convergence.log.

Your final response is EXACTLY one line:
STATUS: {"status":"done","step":"3","chapter":{{chapter_id}},"aggregate_verdict":"pass"|"revise"|"rewrite","verdicts":{"accuracy":"pass"|..., ...}}
```

### 8.16.7 step-4-revision-runner template

```
You are the step-4-revision-runner for chapter {{chapter_id}}. Your sole job is to apply a revision based on the chapter's latest audit verdicts.

Inputs:
- .book-build/parent-state.md
- .book-build/manifest.json (this chapter)
- .book-build/audits/ch{{id_padded}}/*.verdict.json (read all 6 to see which failed)
- .book-build/templates/reviser.md OR .book-build/templates/chapter-writer.md (path only)

Procedure:
1. Read the 6 verdict files. Identify the action:
   - If any verdict is "rewrite" OR pass_counts for any reason ≥ 3, action = "rewrite"
   - Else, action = "revise"
2. Check pass_counts against hard caps (5/reason, 3 rewrites). If a cap would be exceeded, set status="needs_critical_escalation" in manifest, return done without dispatching.
3. Otherwise, dispatch a single worker:
   - For rewrite: dispatch a fresh chapter-writer subagent with the original brief + a "previous attempt failed" note listing failed checks. Do NOT pass the old chapter body.
   - For revise: dispatch a reviser subagent. Pass the existing chapter file path and the failure list.
4. Wait for worker STATUS.
5. Update manifest pass_counts.
6. Append revision event to convergence.log.

Your final response is EXACTLY one line:
STATUS: {"status":"done"|"failed","step":"4","chapter":{{chapter_id}},"action":"revise"|"rewrite"|"escalated"}
```

### 8.16.8 step-5-critical-runner template

```
You are the step-5-critical-runner for round {{round}}. Your sole job is to run the 3-reviewer critical pass and merge outputs.

Inputs:
- .book-build/parent-state.md
- .book-build/manifest.json
- .book-build/templates/critical-reviewer.md (path only)

Procedure:
1. Dispatch 3 critical reviewer subagents in parallel. Each gets a different random sample of chapters (3 per part × 10 parts = 30, split across the 3 reviewers).
2. Wait for 3 STATUS responses. Each reviewer writes its flag list to .book-build/critical-reviews/pass-{{round}}-reviewer-M.json.
3. Read the 3 reviewer JSON files.
4. Merge with the 2-of-3 promotion rule: a chapter is "hard" only if ≥ 2 reviewers flagged it hard. Otherwise soft.
5. Write the merged list to .book-build/critical-reviews/pass-{{round}}-merged.json.
6. Append critical_round event to convergence.log.

Your final response is EXACTLY one line:
STATUS: {"status":"done","step":"5","round":{{round}},"hard_flags":<int>,"soft_flags":<int>}
```

### 8.16.9 step-6-stitch-runner template

```
You are the step-6-stitch-runner for round {{round}}.

Inputs:
- .book-build/parent-state.md
- .book-build/templates/stitch-auditor.md (path only)

Procedure:
1. Dispatch 1 stitch auditor subagent. It reads first/last 200 lines of each chapter and writes .book-build/stitch-verdict.json.
2. Wait for STATUS.
3. Read stitch-verdict.json.
4. Append stitch_round event to convergence.log.

Your final response is EXACTLY one line:
STATUS: {"status":"done","step":"6","round":{{round}},"verdict":"pass"|"revise","issues":<int>}
```

### 8.16.10 step-7-backmatter-runner template

```
You are the step-7-backmatter-runner.

Inputs:
- .book-build/parent-state.md
- .book-build/templates/glossary-writer.md (path)
- .book-build/templates/bibliography-writer.md (path)
- .book-build/templates/concordance-writer.md (path)
- .book-build/templates/back-matter-auditor.md (path)

Procedure:
1. Dispatch 3 back-matter writer subagents in parallel (glossary, bibliography, concordance).
2. Wait for 3 STATUS responses.
3. Create .book-build/audits/back-matter/ directory if it does not exist.
4. Dispatch 3 back-matter-auditor subagents in parallel (one per output, using .book-build/templates/back-matter-auditor.md).
5. Wait for 3 audit STATUS responses.
6. If any audit is "revise", re-dispatch the corresponding writer with the failure list.
7. Update manifest.json back_matter_status fields.
7. Append back_matter events to convergence.log.

Your final response is EXACTLY one line:
STATUS: {"status":"done"|"failed","step":"7","glossary":"done"|"failed","bibliography":"done"|"failed","concordance":"done"|"failed"}
```

### 8.16.11 step-8-final-runner template

```
You are the step-8-final-runner.

Inputs:
- .book-build/parent-state.md
- .book-build/manifest.json
- All files under .book-build/chapters/, .book-build/parts/, .book-build/frontmatter.md, .book-build/back/

Procedure:
1. Verify all chapters in manifest are at status="audited" or "done". If any are at earlier states, return status="failed" with reason "prerequisite_not_met".
2. Read each chapter file and concatenate in the exact order from §7 Step 8:
   frontmatter.md → parts/part-01-intro.md → ch01…ch04 → parts/part-02-intro.md → ch05…ch10 → ... → parts/part-10-intro.md → ch53…ch57 → back/glossary.md → back/bibliography.md → back/concordance.md
3. Write the concatenated output to /home/hwzhang/build/open-claude-code/deep-research-of-cc-source-code.md in a single Write call. If the Write fails due to size, fall back to incremental append via Bash `cat chN.md >> final.md` in order, then verify the final file exists and has non-zero size.
4. Run sanity checks on the final file:
   - wc -w (word count, expect 200000-310000)
   - grep -c '^```mermaid' (expect ≥ 114)
   - grep -cE '^(//|#) src/' (snippet captions, expect ≥ 228)
   - grep -oE 'src/[^ ]+\.tsx?(:L[0-9]+(-L[0-9]+)?)?' | wc -l (citations, expect ≥ 342)
   - grep -cE 'TODO|TBD|\[NEEDS-VERIFY\]|XXX' (forbidden tokens, expect 0)
5. Sample 20 random citations and verify each points to a real line in the repo (use git show with the pinned SHA).
6. Sample 10 random code snippets and diff each against the source.
7. Compile the list of issues (forbidden tokens found, missing sources, snippet drift, etc.).
8. Append final_sanity event to convergence.log with all counts.

Your final response is EXACTLY one line:
STATUS: {"status":"done"|"needs_revisions","step":"8","words":<int>,"diagrams":<int>,"snippets":<int>,"citations":<int>,"forbidden_tokens":<int>,"issues":[<strings>]}
```

---

## 9. Manifest, Terminology, and Status File Schemas

### 9.1 `manifest.json`
```json
{
  "repo_sha": "<git sha>",
  "chapters": [
    {
      "id": 1,
      "id_padded": "01",
      "title": "Why This Book Exists: The Harness Engineering Moment",
      "slug": "why-this-book-exists",
      "part": 1,
      "synopsis": "<string>",
      "source_files": ["src/main.tsx", "src/entrypoints/cli.tsx", "README.md"],
      "her_refs": ["§1 Definition and Core Concept", "§2 Evolution", "§17 TerminalBench 2.0"],
      "diagram_reqs": [
        {"type": "flowchart", "topic": "model + harness = agent block diagram"},
        {"type": "flowchart", "topic": "METR task-duration timeline"}
      ],
      "target_words": 3000,
      "min_words": 2550,
      "max_words": 3750,
      "status": "pending|drafting|drafted|drafting_failed|auditing|audited|needs_critical_escalation|needs_human_review|done",
      "pass_counts": {"accuracy":0,"crossref":0,"diagrams":0,"polish":0,"consistency":0,"gaps":0,"critical":0},
      "last_verdict": null,
      "word_count": null,
      "citations": null,
      "diagram_count": null,
      "snippet_count": null,
      "snippets_verbatim": null,
      "snippets_drift": null,
      "snippets_hallucinated": null,
      "needs_verify": null,
      "brief_checksum": "<md5 of the JSON-serialized brief>"
    },
    ...
  ],
  "parts": [
    { "number": 1, "title": "Foundations and Framing", "chapter_ids": [1,2,3,4], "intro_status": "pending|drafted|audited" },
    ...
  ],
  "frontmatter_status": "pending|drafted|audited",
  "back_matter_status": {
    "glossary": "pending|drafted|audited",
    "bibliography": "pending|drafted|audited",
    "concordance": "pending|drafted|audited"
  },
  "build_rounds": {
    "writing": 0,
    "audit": 0,
    "critical": 0,
    "stitch": 0
  }
}
```

### 9.2 `terminology.json`
```json
{
  "version": 1,
  "terms": [
    { "term": "subagent", "definition": "A single-use agent invoked by the parent session via the Agent tool; has a fresh context; returns one result; does not persist across dispatches." },
    ...
  ]
}
```
New terms proposed by consistency auditors go into `pending_additions` array; parent merges on each audit round.

### 9.3 `convergence.log`
Append-only JSONL, one event per line:
```json
{"ts":"2026-04-10T12:34:56Z","event":"writer_success","chapter":7,"pass":1,"words":5987,"citations":12,"diagrams":3}
{"ts":"...","event":"audit_revise","chapter":7,"pass":"accuracy","issues":2}
{"ts":"...","event":"critical_flag","chapter":14,"severity":"hard","reviewers":2}
{"ts":"...","event":"stitch_pass","round":2,"issues":0}
{"ts":"...","event":"final_sanity","total_words":287500,"diagrams":143,"citations":412,"forbidden_tokens":0}
{"ts":"...","event":"done"}
```

---

## 10. Quality Criteria and Convergence

### 10.1 A chapter is `done` iff ALL of:
1. `status = audited` (or downgraded to `needs_human_review` only if hard caps hit)
2. Latest accuracy verdict = `pass`
3. Latest crossref verdict = `pass`
4. Latest diagrams verdict = `pass`
5. Latest polish verdict = `pass`
6. Latest consistency verdict = `pass`
7. Latest gaps verdict = `pass`
8. `word_count` ∈ [target × 0.85, target × 1.25]
9. `citations ≥ 6` and every citation resolves to a real `src/path.ts` in the repo (sampled in Step 8)
10. `diagram_count ≥ 2` and every mermaid block passed syntax check
11. `snippet_count ≥ 4` and every snippet is verbatim from its referenced file (zero hallucinated, zero drift) and every snippet has a source-path caption comment as its first line
12. At least one snippet appears in the "Data structures and contracts" section
13. At least one snippet appears in the "Control flow" section
14. At least one snippet comes from each of the chapter's top 3 source files in its brief
15. `her_refs ≥ 2` and every ref maps to a real HER section
16. Zero forbidden tokens in body
17. Chapter has no unresolved term conflicts per `terminology.json`
18. Zero unresolved `hard` flags from any critical reviewer
19. All 6 mandatory sections present in correct order
20. Developer takeaways section is 150–300 words, prose paragraph

### 10.2 The book is `done` iff:
- Every chapter is `done`
- `stitch-verdict.json.verdict = "pass"`
- Back-matter statuses all `audited`
- Final stitched file word count ∈ [200,000, 310,000]
- Final file contains ≥ 114 mermaid blocks
- Final file contains ≥ 228 quoted code snippets (fenced blocks whose first line is a source-path caption comment matching `// src/...` or `# src/...`)
- Final file contains ≥ 342 matches of the citation regex
- Two consecutive Step 8 rounds produced zero revision dispatches (i.e., steady-state)
- Zero forbidden tokens in the final stitched file
- Zero `[NEEDS-VERIFY]` markers
- `convergence.log` shows `{"event":"done"}` entry

---

## 11. Risks and Mitigations

| # | Risk | Mitigation |
|---|---|---|
| 1 | Context overflow in writer subagents when reading many source files | Step 0 pre-computes line counts; briefs include hints like "read src/query.ts lines 200–900 for the loop phase"; writers instructed to use offset/limit for files > 1000 LOC |
| 2 | Hallucinated file paths or line numbers | Accuracy auditor re-reads every cited file and verifies citation; Step 8 final sanity does a sampled regex validation |
| 3 | Shallow analysis (not deep research) | Briefs enforce 6 mandatory sections; gaps auditor flags missing depth; critical reviewer samples catch shallowness |
| 4 | Mermaid syntax errors | Diagrams auditor runs pattern-based validator (allowed types, balanced brackets, valid operators); writers restricted to 5 diagram types |
| 5 | Inconsistent voice across subagents | Every writer prompt embeds the same house style block verbatim; polish auditor checks drift; consistency auditor maintains `terminology.json` |
| 6 | GLM 5.1 skipping the audit loop | This plan forbids marking any chapter `done` without all six verdict files present; parent's Step 8 asserts structurally |
| 7 | Infinite polishing loop | Hard caps: 5 passes per reason per chapter, 4 critical reviewer passes, 3 stitch audits, 10 global audit rounds, 3 Step 8 rounds; escape hatch at Step 9 |
| 8 | Writer drifts from brief | `brief_checksum` in writer status JSON; parent verifies against manifest |
| 9 | Subagent produces placeholder text | Forbidden-token grep in Step 8 is the final gate; any occurrence → revision |
| 10 | Duplicate content across chapters | Stitch auditor checks for identical first-5-line duplicates; critical reviewer catches conceptual duplication |
| 11 | Forgotten HER cross-references | Every brief lists exact HER refs; cross-ref auditor verifies one-for-one |
| 12 | File path drift (repo moved files) | Step 0 validates every source_files path before dispatching any writer; missing paths flagged and demoted |
| 13 | Overlapping writes | Parent enforces 1 subagent per file; chapter files are 1:1 with chapters; no subagent edits a file another has in flight |
| 14 | Cost explosion | Pass caps, parallelism caps, token meter in `convergence.log`, escape hatch at 10 rounds |
| 15 | Writer dumps whole book in one chapter | Word upper bound (target × 1.25) → auto-revise |
| 16 | GLM 5.1 invents its own structure | Parent copies chapter table verbatim into manifest.json at Step 0; all dispatches use manifest entries |
| 17 | Critical reviewer too harsh | 2-of-3 promotion rule; soft flags cap at 2 retries; rate-limited to 4 passes |
| 18 | Subagent runs out of tool calls mid-read | Briefs prioritize: top 3 files in full, rest with offset/limit on relevant ranges |
| 19 | Back matter drifts from real body | Back matter is Step 7 AFTER body stable; concordance reads manifest source_files arrays |
| 20 | Final stitched file corrupted by concurrent writes | Parent does Step 8 alone; manifest is frozen before stitch |
| 21 | HER report path unreachable from subagents | Step 0 pre-extracts excerpts to `.book-build/her-excerpts/chNN.md`; writers read excerpts not full HER |
| 22 | Subagent returns no status line | Treated as failure; fresh writer re-dispatched with "previous attempt crashed" note |
| 23 | `terminology.json` grows unbounded | Only consistency auditors propose; parent merges ≤ 5 new terms per round; hard cap 200 total terms |
| 24 | Partial run interrupted | `manifest.json` is source of truth; on resume, parent skips any chapter with `status="audited"` or `"done"` and picks up remaining work |
| 25 | Stitch Step 8 grows too large for single Write | Parent writes in a single call; if Write fails on size, fallback is incremental Append via Bash `cat` (one chapter at a time) with final verification |
| 26 | Hallucinated code snippets (writer invents code that doesn't exist) | Every snippet has a mandatory source-path caption on its first line; accuracy auditor re-reads the referenced range and compares character-by-character; any `snippet_hallucinated > 0` triggers rewrite |
| 27 | Code snippets drift from source after repo changes | Writers pin to `repo-sha.txt`; snippet sampling in Step 8 uses `git show <sha>:<path>` if live repo has drifted; drift → targeted reviser |
| 28 | Writers paraphrase code instead of quoting verbatim | Writer template forbids paraphrase; accuracy auditor's `drift` classification catches near-misses; revise verdict triggers reviser to replace with verbatim copy |
| 29 | Chapters have 4 snippets but all from the same file | Gaps auditor checks `top_files_without_snippets` — at least one snippet must come from each of the top 3 files in the brief |
| 30 | Oversized snippets blow up token budget | Polish auditor flags snippets > 60 lines; writer template caps at 8–30 typical, 60 absolute |
| 31 | **Parent context auto-compacts mid-run and loses critical rules** | Step 0.5 extracts all 15 subagent templates to `.book-build/templates/` and seeds `parent-state.md` with compressed critical rules. Parent re-reads `parent-state.md` after every compaction per §13.2. Plan file is not re-read after Step 0.5. |
| 32 | Parent reads subagent response prose, bloating context per dispatch | §8.0 rule 1: every subagent's final response is exactly one line — the `STATUS: {...}` JSON. No prose before or after. Parent context stays flat across dispatches. |
| 33 | Parent reads chapter bodies or source files directly | §5.5.4 forbids the parent from reading any chapter file or any source file under `src/**`. Only auditors and writers read those, in their fresh contexts. |
| 34 | Parent fails to monitor its own context usage | §5.5.6 gives explicit thresholds (25/40/55/75%) and mandates `/compact` with the exact preservation prompt at each threshold. Parent checks the status-line indicator after every dispatch round. |
| 35 | Templates drift between Sections 8.x and `templates/*.md` files | Step 0.5 is verbatim extraction. If the plan's Section 8 is later updated, Step 0.5 must be re-run to regenerate the template files. The convergence log records the `templates_extracted` event; any later plan edit to §8 requires a new extraction event. |

---

## 12. Verification Protocol

After `status="done"` in `convergence.log`, the user (or an independent reviewer) can verify the book as follows:

1. **File exists**: `ls -l /home/hwzhang/build/open-claude-code/deep-research-of-cc-source-code.md`
2. **Word count**: `wc -w <file>` returns ∈ [200000, 310000]
3. **Mermaid count**: `grep -c '^```mermaid' <file>` returns ≥ 114
4. **Snippet count**: `grep -cE '^(//|#) src/' <file>` returns ≥ 228 (counts snippet caption lines)
5. **Citation count**: `grep -oE 'src/[^ ]+\.tsx?(:L[0-9]+(-L[0-9]+)?)?' <file> | wc -l` returns ≥ 342
6. **No forbidden tokens**: `grep -cE 'TODO|TBD|\[NEEDS-VERIFY\]' <file>` returns 0
7. **TOC sanity**: first 300 lines contain "Chapter 1" through "Chapter 57" and "Part I" through "Part X"
8. **Chapter sampling**: pick 5 random chapters from different parts, spot-check that each contains the 6 mandatory sections
9. **Citation sampling**: pick 20 random citations, verify each `src/path.ts:Lnnn` points to a real line in the current repo (note: the book was written against `repo-sha.txt`; if repo has changed, use `git show <sha>:<path>` to verify)
10. **Snippet sampling**: pick 10 random quoted code snippets, read the referenced range from the repo, diff against the snippet body (ignoring `// ...` trim markers and the caption). Zero diffs expected.
11. **HER cross-reference sampling**: pick 10 random chapters, verify each has at least one "Where cc diverges from the published pattern" section with ≥ 150 words

If all 11 checks pass, the book is considered successfully delivered.

---

## 13. Resume Protocol (if GLM 5.1 is interrupted, compacted, or re-launched)

Two resume paths: (A) **external resume** (user Ctrl+C'd and relaunched `fcoder`), and (B) **mid-session compaction recovery** (auto-compact or manual `/compact` fired; parent context was flushed but session is alive). Both paths use the same state files; the only difference is which files to re-read first.

### 13.1 Path A: External resume
The parent is a brand-new session with no memory. On the first turn:
1. Check `.book-build/manifest.json` exists. If not → run Step 0 and Step 0.5 fresh.
2. Check `.book-build/parent-state.md` exists. If not → run Step 0.5 (extraction of templates and state).
3. Check `.book-build/templates/` has 27 files (16 worker + 11 step-runner). If any missing → re-run Step 0.5.
4. Read `parent-state.md` to rehydrate rules.
5. Read `manifest.json` to rehydrate chapter state.
6. Check `repo-sha.txt`. Compare to current `git rev-parse HEAD`. If drift → log warning; writers' citations may be stale; note to user in next status message.
7. For each chapter, proceed per §13.3 status decision table.
8. Check back-matter statuses; dispatch only missing ones.
9. If all chapters audited and back matter done → go straight to Step 8.

### 13.2 Path B: Mid-session compaction recovery
The parent just finished a `/compact` (either user-initiated via Shift+Tab → Compact, or auto-triggered at 95%). Whatever summary remained in context should already include "I am orchestrating book-implementation-plan.md". The parent's very first post-compaction action:
1. `Read .book-build/parent-state.md` — rehydrate rules.
2. `Read .book-build/manifest.json` — rehydrate chapter state.
3. Check if `parent-state.md` has a valid `current_step` field. If the field says "Step 1 in progress with N chapters drafted", resume dispatching remaining writers.
4. If any in-flight dispatch IDs were recorded before compaction and are still live (the Agent tool can list background agents), resume monitoring them. Otherwise, assume they were completed or lost, and check `manifest.json` to see which chapters have `status="drafted"` as ground truth.
5. Do NOT re-dispatch a writer for a chapter whose file already exists under `chapters/` AND whose manifest status is `drafted` or later. Overwriting wastes work.
6. After rehydration, proceed per §13.3.

### 13.3 Status decision table (both paths)
For each chapter in `manifest.json`:

| Current status | Action |
|---|---|
| `pending` | Enter Step 1 (dispatch writer) |
| `drafting` | Check if the writer is still in flight (Agent tool list). If yes → monitor. If no → re-dispatch fresh writer. |
| `drafting_failed` | Re-dispatch fresh writer |
| `drafted` | Enter Step 3 (dispatch 6 auditors) |
| `auditing` | Dispatch only the specific auditors whose `*.verdict.json` files don't yet exist |
| `audited` | Skip; it's done for the writing phase (may still be touched by Step 5 critical review or Step 6 stitch) |
| `needs_critical_escalation` | Enter Step 5 for this chapter |
| `needs_human_review` | Skip; leave the placeholder note in the chapter file; log to `convergence-failures.md` at end of run |
| `done` | Skip |

### 13.4 Idempotency guarantee
Every step is safe to re-run. Writers overwrite chapter files; auditors overwrite verdict files; revisers overwrite the chapter in place. No append-only corruption. `convergence.log` is append-only but safe (it's a log, not state).

### 13.5 Updating `parent-state.md` after resume
After any resume, the parent updates `parent-state.md`'s `current_step` field to reflect actual state derived from `manifest.json`. This keeps the snapshot honest for the next resume.

---

## 14. Final Notes to GLM 5.1

- **Do not invent structure.** The chapter table in Section 6 is the spec. Copy it verbatim.
- **Do not skip the audit loop.** Every chapter must pass all 6 audit types before it counts as done.
- **Do not read chapter bodies in the parent session.** Read only verdicts, manifests, and status files. Your context must stay clean.
- **Do Step 0.5 before any writer dispatch.** Extract templates to `.book-build/templates/` and write `parent-state.md`. This is how you survive the multi-hour run without losing critical rules to compaction. Do not skip Step 0.5 even if it feels redundant.
- **Watch your context usage.** Check the status-line indicator after every batch dispatch. At 55% start planning a compact; at 75% compact now. Use the exact `/compact` prompt from §5.5.3 — do NOT let it summarize freely.
- **After every compaction, re-read `.book-build/parent-state.md` FIRST.** Before anything else. It rehydrates your rules.
- **Use fresh subagents every time.** Never resume a previous subagent.
- **Be patient.** This book will take many audit rounds. That is expected and correct. Do not shortcut the loop.
- **Show the code.** A book about a codebase that doesn't quote the codebase isn't worth writing. Writers must include ≥ 4 verbatim code snippets per chapter, each with a source-path caption and an explanation that references specific identifiers from the snippet. Paraphrasing code instead of quoting it is a rewrite-triggering failure.
- **Verbatim means verbatim.** Whitespace, identifiers, punctuation — exactly as in the source file. The accuracy auditor character-compares every snippet to the pinned repo.
- **When in doubt, prefer precision over prose.** Cite the code, quote the code, and explain the code. Don't rhapsodize about it.
- **Obey the forbidden token list.** Even one "simply" can trigger a revision; make your writers avoid them upfront.
- **Respect the escape hatch.** Do not loop forever. If convergence is impossible, halt, write `convergence-failures.md`, and report to the user.
- **The user wants a book that teaches a future AI (or engineer) how cc works well enough to build a better successor.** Every chapter should leave the reader with: (1) a clear mental model of the subsystem, (2) file-level evidence for every claim, (3) **actual code excerpts** that let the reader see what cc does without opening the repo, (4) a connection to HER patterns and failure modes, (5) concrete takeaways for building a new long-running agent. If a draft doesn't deliver all five, it isn't done.

---

## Appendix: Critical file reading order for GLM 5.1 (high priority)

The most load-bearing files in the cc codebase, in rough reading priority for the writers of the highest-value chapters:

1. `/home/hwzhang/build/open-claude-code/src/query.ts` (Chapter 7)
2. `/home/hwzhang/build/open-claude-code/src/services/api/claude.ts` (Chapter 8)
3. `/home/hwzhang/build/open-claude-code/src/Tool.ts` (Chapter 11)
4. `/home/hwzhang/build/open-claude-code/src/services/tools/toolExecution.ts` (Chapter 12)
5. `/home/hwzhang/build/open-claude-code/src/tools/AgentTool/AgentTool.tsx` (Chapter 18)
6. `/home/hwzhang/build/open-claude-code/src/utils/permissions/permissions.ts` (Chapter 32)
7. `/home/hwzhang/build/open-claude-code/src/services/compact/compact.ts` (Chapter 28)
8. `/home/hwzhang/build/open-claude-code/src/memdir/memdir.ts` (Chapter 26)
9. `/home/hwzhang/build/open-claude-code/src/schemas/hooks.ts` (Chapter 36)
10. `/home/hwzhang/build/open-claude-code/src/services/mcp/client.ts` (Chapter 40)
11. `/home/hwzhang/build/open-claude-code/src/coordinator/coordinatorMode.ts` (Chapter 21)
12. `/home/hwzhang/build/open-claude-code/src/utils/tasks.ts` (Chapter 22)
13. `/home/hwzhang/build/open-claude-code/src/utils/planModeV2.ts` (Chapter 45)
14. `/home/hwzhang/build/open-claude-code/src/main.tsx` (Chapter 6)
15. `/home/hwzhang/build/open-claude-code/src/entrypoints/init.ts` (Chapter 5)

These files cover the harness spine. Writers of other chapters should read the files listed in their specific chapter brief.

---

## Appendix B: Immediate Intervention Script (for an already-running session)

If the context management rules (Section 5.5) are added to the plan **after** a run has already started — for example, the parent is already mid-Step-1 at 75% context usage — the user can paste the following block into the live `fcoder` session to retrofit context hygiene without losing progress. This must be pasted **while the current batch of writers finishes but before dispatching the next batch**.

> **Context hygiene retrofit.** Plan has been updated with new Section 5.5 (Parent Context Management) and Step 0.5 (Template extraction). Do the following immediately, in order, before dispatching any more writers:
>
> 1. **Wait for all in-flight writers to finish** (do not cancel them). Collect their STATUS lines into manifest as you normally would.
> 2. **Do not dispatch the next writer batch yet.**
> 3. **Execute Step 0.5 retroactively**: extract Sections 8.1 through 8.15.1 of the plan file (`/home/hwzhang/build/open-claude-code/book-implementation-plan.md`) to `.book-build/templates/*.md` — one file per section, using the 16 filenames listed in Section 5.5.4 of the updated plan. Read Section 5.5 of the updated plan first for the exact filename list and rules.
> 4. **Write `.book-build/parent-state.md`** per Section 5.5.2. Include: current step tracker (Step 1 in progress, N/57 chapters drafted where N is the actual count from manifest), hard caps, forbidden tokens, convergence criteria summary, template file paths, key directory paths, parallelism caps, dispatch rules. Target ~1,000 words.
> 5. **Append to `convergence.log`** a `templates_extracted` event and a `context_retrofit_applied` event.
> 6. **Run `/compact` now** using the exact preservation prompt from Section 5.5.3 of the updated plan. After compaction, your first action must be `Read .book-build/parent-state.md` then `Read .book-build/manifest.json` to rehydrate state.
> 7. **Only after rehydration**, resume dispatching writers for the remaining chapters. When dispatching, do NOT embed the template body inline — pass the template file path (e.g., `.book-build/templates/chapter-writer.md`) and instruct the subagent to Read it first.
> 8. **Going forward**: after every batch of writers or auditors completes, check your context usage. At >55% plan a compact; at >75% compact immediately using the same preservation prompt. Re-read `parent-state.md` after every compaction.
>
> Update `parent-state.md`'s `current_step` field after every major state change so future resumes have an honest snapshot.
>
> Do not re-draft any chapter that already has a file under `.book-build/chapters/` and `status="drafted"` in manifest. Those chapters will be audited later in Step 3 and rewritten there if needed.

---

**End of Plan.**
