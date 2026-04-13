# Parent State Snapshot

## A. Current Step Tracker

Step 2 complete. Front matter written (1395 words). All 10 part intros written (582-808 words each). manifest.json updated with frontmatter_status=done and parts[N].intro_status=done. Step 1 complete. 57/57 chapters drafted.

## B. Hard Caps

- 5 passes per reason per chapter (accuracy, crossref, diagrams, polish, consistency, gaps)
- 3 rewrites per chapter (full chapter-writer re-dispatch)
- 4 critical reviewer passes (step-5-critical-runner may run 4 rounds)
- 3 stitch rounds (step-6-stitch-runner may run 3 rounds)
- 10 total audit rounds (the outer loop of steps 3-5 may repeat up to 10 times)
- 3 Step 8 rounds (step-8-final-runner may be dispatched 3 times)
- If any cap is exceeded for a chapter, set status="needs_critical_escalation" and skip further work on it
- If 10 audit rounds pass without convergence, write .book-build/convergence-failures.md and halt

## C. Forbidden Tokens

The following tokens are forbidden in all chapter output and will trigger automatic revision if detected: TODO, TBD, XXX, [NEEDS-VERIFY], "similar to chapter", "as noted earlier", "we will discuss", "as we shall see", "simply" (standalone), "obviously", "just" (as filler word), "in summary" (as phrase). Any occurrence in a drafted chapter must be flagged by the Polish Auditor and corrected by the Reviser before the chapter can pass audit.

## D. Convergence Criteria

A chapter is done iff all 6 audits (accuracy, crossref, diagrams, polish, consistency, gaps) produce a "pass" verdict, word count is within [min_words, max_words], citation count is at least 6, diagram count is at least 2, snippet count is at least 4, all snippets are verified verbatim, and no forbidden tokens are present. The book is done iff all 57 chapters are at status "done" or "audited", the stitch audit passes with zero issues, all 3 back-matter files are audited and passing, and two consecutive Step 8 rounds produce zero revision dispatches.

## E. Step-Runner Template Paths

1. /home/hwzhang/build/open-claude-code/.book-build/templates/step-0-runner.md
2. /home/hwzhang/build/open-claude-code/.book-build/templates/step-0.5-runner.md
3. /home/hwzhang/build/open-claude-code/.book-build/templates/step-0.9-runner.md
4. /home/hwzhang/build/open-claude-code/.book-build/templates/step-1-batch-runner.md
5. /home/hwzhang/build/open-claude-code/.book-build/templates/step-2-runner.md
6. /home/hwzhang/build/open-claude-code/.book-build/templates/step-3-chapter-runner.md
7. /home/hwzhang/build/open-claude-code/.book-build/templates/step-4-revision-runner.md
8. /home/hwzhang/build/open-claude-code/.book-build/templates/step-5-critical-runner.md
9. /home/hwzhang/build/open-claude-code/.book-build/templates/step-6-stitch-runner.md
10. /home/hwzhang/build/open-claude-code/.book-build/templates/step-7-backmatter-runner.md
11. /home/hwzhang/build/open-claude-code/.book-build/templates/step-8-final-runner.md

## F. Worker Template Paths

1. /home/hwzhang/build/open-claude-code/.book-build/templates/chapter-writer.md
2. /home/hwzhang/build/open-claude-code/.book-build/templates/frontmatter-writer.md
3. /home/hwzhang/build/open-claude-code/.book-build/templates/part-intro-writer.md
4. /home/hwzhang/build/open-claude-code/.book-build/templates/accuracy-auditor.md
5. /home/hwzhang/build/open-claude-code/.book-build/templates/crossref-auditor.md
6. /home/hwzhang/build/open-claude-code/.book-build/templates/diagrams-auditor.md
7. /home/hwzhang/build/open-claude-code/.book-build/templates/polish-auditor.md
8. /home/hwzhang/build/open-claude-code/.book-build/templates/consistency-auditor.md
9. /home/hwzhang/build/open-claude-code/.book-build/templates/gaps-auditor.md
10. /home/hwzhang/build/open-claude-code/.book-build/templates/reviser.md
11. /home/hwzhang/build/open-claude-code/.book-build/templates/critical-reviewer.md
12. /home/hwzhang/build/open-claude-code/.book-build/templates/stitch-auditor.md
13. /home/hwzhang/build/open-claude-code/.book-build/templates/glossary-writer.md
14. /home/hwzhang/build/open-claude-code/.book-build/templates/bibliography-writer.md
15. /home/hwzhang/build/open-claude-code/.book-build/templates/concordance-writer.md
16. /home/hwzhang/build/open-claude-code/.book-build/templates/back-matter-auditor.md

## G. Key Directory Paths

- Manifest: /home/hwzhang/build/open-claude-code/.book-build/manifest.json
- Convergence log: /home/hwzhang/build/open-claude-code/.book-build/convergence.log
- Chapters: /home/hwzhang/build/open-claude-code/.book-build/chapters/
- Audits: /home/hwzhang/build/open-claude-code/.book-build/audits/
- HER excerpts: /home/hwzhang/build/open-claude-code/.book-build/her-excerpts/
- Templates: /home/hwzhang/build/open-claude-code/.book-build/templates/
- Parent state: /home/hwzhang/build/open-claude-code/.book-build/parent-state.md
- Loc hints: /home/hwzhang/build/open-claude-code/.book-build/loc-hints/files.json
- Terminology: /home/hwzhang/build/open-claude-code/.book-build/terminology.json
- Front matter: /home/hwzhang/build/open-claude-code/.book-build/frontmatter.md
- Parts: /home/hwzhang/build/open-claude-code/.book-build/parts/
- Back matter: /home/hwzhang/build/open-claude-code/.book-build/back/
- Critical reviews: /home/hwzhang/build/open-claude-code/.book-build/critical-reviews/

## H. Parallelism Caps

- 2 step-1-batch-runners in flight simultaneously (each handling up to 4 chapters = 8 chapters being drafted concurrently)
- 2 step-3-chapter-runners in flight simultaneously (auditing 2 chapters concurrently, each running 6 auditors)
- 3 critical reviewers running inside step-5-critical-runner (the step-5 runner dispatches 3 reviewers in parallel)
- 1 stitch runner (step-6-stitch-runner is single-instance)
- 1 final runner (step-8-final-runner is single-instance)
