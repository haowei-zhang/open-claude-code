# Parent State Snapshot

## A. Current Step Tracker

Step 0.5 complete. Step 0.9 verification-gate passed. Step 1 in progress. 4/57 chapters drafted (chapters 1-4). No audit rounds completed. No rewrites consumed. No stitch rounds run. Batch 1 of step 1 complete; remaining 53 chapters pending. Next action is step-1 batch drafting for chapters 5-8.

## B. Hard Caps

These limits are absolute and may not be exceeded without manual override recorded in convergence.log:

- 5 passes per reason per chapter. The seven audit reasons are: accuracy, crossref, diagrams, polish, consistency, gaps, critical. Each reason independently tracks its pass count. Once a reason hits 5 passes for a given chapter, no further passes of that reason are permitted for that chapter unless an override is logged.
- 3 rewrites per chapter before escalation. A rewrite is a full chapter re-draft triggered by a revise verdict that cannot be resolved by targeted patching. After 3 rewrites, the chapter escalates to a manual review queue.
- 4 critical reviewer passes (step-5 rounds). The critical reviewer is a separate role from the six standard auditors. It may run at most 4 times per chapter.
- 3 stitch rounds (step-6 rounds). Stitching joins part-intro text, chapter transitions, and cross-references across adjacent chapters. At most 3 full stitch passes.
- 10 total audit rounds across all chapters. An audit round is one sweep of the six auditors (accuracy, crossref, diagrams, polish, consistency, gaps) over all chapters. After 10 rounds, the book enters forced-convergence regardless of remaining issues.
- 3 Step 8 rounds (final assembly attempts). Step 8 produces the final manuscript. If it fails 3 times, a human must intervene.

## C. Forbidden Tokens

The following tokens and phrases must never appear in any chapter draft, audit report, or generated prose. If any auditor detects one, the verdict is automatically fail with reason forbidden_token:

- TODO
- TBD
- XXX
- [NEEDS-VERIFY]
- "similar to chapter" (vague cross-reference)
- "as noted earlier" (lazy back-reference without section anchor)
- "we will discuss" (forward promise without chapter/section pointer)
- "as we shall see" (hand-wave forward reference)
- "simply" (minimizing filler)
- "obviously" (assumption of shared knowledge)
- "just" (when used as filler word, not as synonym for "only" in technical sense)
- "in summary" (as phrase; the word "summary" alone in other contexts is permitted)

## D. Convergence Criteria

A chapter is done if and only if all of the following hold simultaneously:

1. All 6 standard audits (accuracy, crossref, diagrams, polish, consistency, gaps) return "pass".
2. Word count falls within [min_words, max_words] as defined in manifest.json for that chapter.
3. Citation count is at least 6.
4. Diagram count is at least 2.
5. Snippet count is at least 4.
6. Every code snippet is verbatim: snippets_verbatim equals snippets_drift equals true, snippets_hallucinated equals zero.

The book is done if and only if all of the following hold simultaneously:

1. All 57 chapters are done per the chapter criteria above.
2. The stitch audit passes (cross-chapter transitions are coherent and free of redundancy).
3. All three back-matter files (glossary, bibliography, concordance) are audited as pass.
4. Two consecutive zero-revision sweeps occur: after a full audit round, no chapters need revise or rewrite.

## E. Step-Runner Template Paths

These 11 templates orchestrate each build step. Each runner template defines the step's inputs, outputs, pass/fail criteria, and delegation to worker templates:

1. `.book-build/templates/step-0-runner.md` -- initial setup and directory creation
2. `.book-build/templates/step-0.5-runner.md` -- manifest population and canonical corrections
3. `.book-build/templates/step-0.9-runner.md` -- verification gate before drafting begins
4. `.book-build/templates/step-1-batch-runner.md` -- batch chapter drafting (dispatches chapter-writer workers)
5. `.book-build/templates/step-2-runner.md` -- first-pass audit sweep (dispatches all six auditors)
6. `.book-build/templates/step-3-chapter-runner.md` -- per-chapter revision based on audit results
7. `.book-build/templates/step-4-revision-runner.md` -- targeted patching and re-audit loop
8. `.book-build/templates/step-5-critical-runner.md` -- critical reviewer pass (dispatches critical-reviewer workers)
9. `.book-build/templates/step-6-stitch-runner.md` -- cross-chapter stitching and transition audit
10. `.book-build/templates/step-7-backmatter-runner.md` -- back-matter generation and audit
11. `.book-build/templates/step-8-final-runner.md` -- final manuscript assembly

## F. Worker Template Paths

These 16 templates define the behavior of individual workers invoked by step-runners:

1. `.book-build/templates/chapter-writer.md` -- drafts a single chapter from manifest entry, HER excerpts, and source files
2. `.book-build/templates/frontmatter-writer.md` -- writes preface, acknowledgments, and front matter
3. `.book-build/templates/part-intro-writer.md` -- writes part introduction pages linking chapter themes
4. `.book-build/templates/accuracy-auditor.md` -- verifies factual correctness of code references and technical claims
5. `.book-build/templates/crossref-auditor.md` -- checks cross-references between chapters resolve correctly
6. `.book-build/templates/diagrams-auditor.md` -- validates diagram syntax, labels, and completeness
7. `.book-build/templates/polish-auditor.md` -- checks prose quality, style compliance, and forbidden tokens
8. `.book-build/templates/consistency-auditor.md` -- checks terminology, naming conventions, and tone consistency
9. `.book-build/templates/gaps-auditor.md` -- identifies missing coverage of declared source files and HER references
10. `.book-build/templates/reviser.md` -- applies targeted patches to chapters based on audit verdicts
11. `.book-build/templates/critical-reviewer.md` -- performs adversarial review for logical errors and unsupported claims
12. `.book-build/templates/stitch-auditor.md` -- audits cross-chapter transitions for coherence and redundancy
13. `.book-build/templates/glossary-writer.md` -- generates the glossary from terminology.json
14. `.book-build/templates/bibliography-writer.md` -- generates the bibliography from HER references
15. `.book-build/templates/concordance-writer.md` -- generates the concordance index
16. `.book-build/templates/back-matter-auditor.md` -- audits all three back-matter files

## G. Key Directory Paths

All paths are relative to the repository root:

- Manifest: `.book-build/manifest.json` -- canonical chapter metadata, source files, status, and pass counts
- Convergence log: `.book-build/convergence.log` -- append-only event log for all build state transitions
- Chapters: `.book-build/chapters/` -- one subdirectory per chapter (e.g., `01-why-this-book-exists/`)
- Audits: `.book-build/audits/` -- audit results organized by chapter and reason
- HER excerpts: `.book-build/her-excerpts/` -- extracted relevant sections from the Harness Engineering Report
- Templates: `.book-build/templates/` -- all runner and worker templates
- Parent state: `.book-build/parent-state.md` -- this file
- LOC hints: `.book-build/loc-hints/files.json` -- line-of-code hints for source files used in snippet selection
- Terminology: `.book-build/terminology.json` -- canonical term definitions for consistency checking
- Repo SHA: `.book-build/repo-sha.txt` -- pinned commit hash for reproducibility
- Parts: `.book-build/parts/` -- part introduction drafts
- Back matter: `.book-build/back/` -- glossary, bibliography, and concordance drafts
- Critical reviews: `.book-build/critical-reviews/` -- critical reviewer outputs per chapter

## H. Parallelism Caps

These caps prevent resource exhaustion and ensure deterministic build ordering:

- 2 step-1-batch-runners in flight simultaneously. Each batch-runner dispatches multiple chapter-writer workers. Capping at 2 prevents context-window contention across too many concurrent drafting agents.
- 2 step-3-chapter-runners in flight simultaneously. Revision runners each handle one chapter. Capping at 2 ensures revision quality does not degrade under parallel load.
- 3 critical reviewers dispatched inside step-5-critical-runner. The critical reviewer is the most compute-intensive worker because it re-reads the full chapter and its source files. Three concurrent instances balance throughput against memory pressure.
- 1 stitch runner (step-6) at a time. Stitching requires a consistent view of all chapters; parallelism would create merge conflicts in cross-chapter references.
- 1 final runner (step-8) at a time. Final assembly is inherently sequential: it concatenates all chapters, inserts back matter, and produces a single output file.
