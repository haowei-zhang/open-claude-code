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
# Universal rules (apply to every subagent)

1. **Your final response is exactly one line.** That line is the `STATUS: {...}` JSON. No prose, no narration, no tool-call echoes, no explanations, no markdown headings, nothing before or after. The parent receives this line as your sole output and any extra bytes bloat its context.
2. **Do all your work via file writes and intermediate tool calls.** If you want to explain what you did, write it to a file under `.book-build/` (e.g., the audit prose reports go to `.md` files, chapter bodies go to chapter files). Do NOT put explanations in your final response.
