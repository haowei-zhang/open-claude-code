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
8. Append back_matter events to convergence.log.

Your final response is EXACTLY one line:
STATUS: {"status":"done"|"failed","step":"7","glossary":"done"|"failed","bibliography":"done"|"failed","concordance":"done"|"failed"}
