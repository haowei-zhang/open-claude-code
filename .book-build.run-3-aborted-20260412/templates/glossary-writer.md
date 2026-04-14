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
