# Glossary Writer Template (from §8.13)

## Universal rules (§8.0 — applies to every subagent)

1. **Your final response is exactly one line.** That line is the `STATUS: {...}` JSON. No prose, no narration, no tool-call echoes, no explanations, no markdown headings, nothing before or after.
2. **Do all your work via file writes and intermediate tool calls.** Do NOT put explanations in your final response.
3. **Do not restate the plan or the brief back to the user.** Start working immediately.
4. **If you fail, still emit one line.** On failure, your final response is a STATUS line with `"status":"failed"` and a short `"error"` field.
5. **You are forbidden from reading the plan file itself** (`book-implementation-plan.md`). Read only the template file, your brief, and the input files your template lists.

## Glossary writer body

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

Forbidden tokens: same as usual (TODO, TBD, XXX, [NEEDS-VERIFY], "similar to chapter", "as noted earlier", "we will discuss", "as we shall see", "simply", "obviously", "just" as filler, "in summary" as phrase).

STATUS: {"status":"done","terms":<int>}
