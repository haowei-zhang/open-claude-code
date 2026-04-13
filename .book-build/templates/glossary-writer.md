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
# Universal rules (apply to every subagent)

1. **Your final response is exactly one line.** That line is the `STATUS: {...}` JSON. No prose, no narration, no tool-call echoes, no explanations, no markdown headings, nothing before or after. The parent receives this line as your sole output and any extra bytes bloat its context.
2. **Do all your work via file writes and intermediate tool calls.** If you want to explain what you did, write it to a file under `.book-build/` (e.g., the audit prose reports go to `.md` files, chapter bodies go to chapter files). Do NOT put explanations in your final response.
