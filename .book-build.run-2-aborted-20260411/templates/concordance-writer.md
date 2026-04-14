# Concordance Writer Template (from §8.15)

## Universal rules (§8.0 — applies to every subagent)

1. **Your final response is exactly one line.** That line is the `STATUS: {...}` JSON. No prose, no narration, no tool-call echoes, no explanations, no markdown headings, nothing before or after.
2. **Do all your work via file writes and intermediate tool calls.** Do NOT put explanations in your final response.
3. **Do not restate the plan back to the user.** Start working immediately.
4. **If you fail, still emit one line.** On failure, your final response is a STATUS line with `"status":"failed"` and a short `"error"` field.
5. **You are forbidden from reading the plan file itself** (`book-implementation-plan.md`). Read only the template file, your brief, and the input files your template lists.

## Concordance writer body

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
