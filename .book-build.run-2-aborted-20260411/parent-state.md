# Parent State — Compressed Reference

**Updated**: Step 0.5 complete. Ready for Step 1 (chapter writing).

## 1. Current Step Tracker

Step 0 done. Step 0.5 done. Step 1 pending — dispatch writers for Part I (Ch 1–4) first, then batch Parts II–X four chapters at a time. 0/57 drafted.

## 2. Hard Caps

- 5 passes per audit reason per chapter (accuracy, crossref, diagrams, polish, consistency, gaps)
- 3 total rewrites per chapter
- 4 critical reviewer passes total
- 3 stitch audit passes
- 10 total audit rounds globally
- 3 Step 8 (final sanity) rounds
- 200 total terms cap in terminology.json; ≤5 new terms merged per round

## 3. Forbidden Tokens

TODO, TBD, XXX, [NEEDS-VERIFY], "similar to chapter", "as noted earlier", "we will discuss", "as we shall see", "simply" (standalone), "obviously", "just" (as filler), "in summary" (as phrase)

## 4. Convergence Criteria

A chapter is `done` iff: status=audited, all six latest verdicts=pass, word_count in [target×0.85, target×1.25], citations≥6, diagrams≥2, snippets≥4 all verbatim, ≥1 snippet in Data Structures section, ≥1 snippet in Control Flow section, ≥1 snippet from each top-3 source file, her_refs≥2, zero forbidden tokens, zero term conflicts, zero hard critical flags, all 6 mandatory sections present, Developer takeaways 150–300 words prose paragraph.

The book is `done` iff: every chapter done, stitch-verdict=pass, back-matter all audited, final word count in [200k, 310k], ≥114 mermaid blocks, ≥228 snippets, ≥342 citations, two consecutive audit sweeps with zero revisions, zero forbidden tokens, convergence.log has `{"event":"done"}`.

## 5. Template File Paths

1. `.book-build/templates/chapter-writer.md` (§8.1)
2. `.book-build/templates/frontmatter-writer.md` (§8.2)
3. `.book-build/templates/part-intro-writer.md` (§8.3)
4. `.book-build/templates/accuracy-auditor.md` (§8.4)
5. `.book-build/templates/crossref-auditor.md` (§8.5)
6. `.book-build/templates/diagrams-auditor.md` (§8.6)
7. `.book-build/templates/polish-auditor.md` (§8.7)
8. `.book-build/templates/consistency-auditor.md` (§8.8)
9. `.book-build/templates/gaps-auditor.md` (§8.9)
10. `.book-build/templates/reviser.md` (§8.10)
11. `.book-build/templates/critical-reviewer.md` (§8.11)
12. `.book-build/templates/stitch-auditor.md` (§8.12)
13. `.book-build/templates/glossary-writer.md` (§8.13)
14. `.book-build/templates/bibliography-writer.md` (§8.14)
15. `.book-build/templates/concordance-writer.md` (§8.15)

## 6. Key Directory Paths

- Manifest: `.book-build/manifest.json`
- Convergence log: `.book-build/convergence.log`
- Chapters: `.book-build/chapters/`
- Audits: `.book-build/audits/`
- HER excerpts: `.book-build/her-excerpts/`
- Templates: `.book-build/templates/`
- Parent state: `.book-build/parent-state.md`
- Terminology: `.book-build/terminology.json`
- Loc hints: `.book-build/loc-hints/files.json`
- Repo SHA: `.book-build/repo-sha.txt`
- Stitch verdict: `.book-build/stitch-verdict.json`
- Critical reviews: `.book-build/critical-reviews/`
- Parts: `.book-build/parts/`
- Back matter: `.book-build/back/`
- Frontmatter: `.book-build/frontmatter.md`
- Final book: `deep-research-of-cc-source-code.md`

## 7. Parallelism Caps

- Writers: max 4 concurrent
- Auditors: max 12 concurrent globally (typical: 2 chapters × 6 types)
- Critical reviewers: 3 concurrent per round
- Revisers: up to 4 concurrent (share 4-writer cap)
- Stitch and backmatter: sequential; 1 each at a time

## 8. Dispatch Rules

- Parent NEVER embeds chapter bodies in prompts — only file paths and verdict JSON
- Every subagent is fresh — no subagent resumes previous context
- Parent reads ONLY: manifest.json, *.verdict.json, terminology.json, convergence.log, parent-state.md, stitch-verdict.json, repo-sha.txt, loc-hints/files.json, and during Step 8 the final assembled output
- Subagents that need another chapter's text read it directly from chapters/
- Every prompt template comes from .book-build/templates/ — parent passes template PATH, not body
- No subagent reads more than ~25 source files
- Every subagent's final response is exactly one line — the STATUS JSON
- Parent NEVER reads any source file under src/** or any full chapter file (except Step 8 grep)
- After compaction, parent's first action is Read parent-state.md then Read manifest.json
- Compaction thresholds: 55-65% consider, 65-75% compact at safe moment, 75-85% compact immediately, >85% emergency compact

## Missing Files (known at bootstrap)

- Ch 3,4: package.json, tsconfig.json, bunfig.toml not in repo (meta-chapters, skip)
- Ch 25: src/types/message.ts missing (use src/utils/messages.ts instead)
- Ch 28: reactiveCompact.ts → apiMicrocompact.ts; cachedMicrocompact.ts → sessionMemoryCompact.ts; contextCollapse/ missing
- Ch 49: assistant/index.ts and gate.ts missing (only sessionHistory.ts exists)
- Ch 50: growthbook.js → growthbook.ts; cost-tracker.ts at src/cost-tracker.ts not src/utils/
