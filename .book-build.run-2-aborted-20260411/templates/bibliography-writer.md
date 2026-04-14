# Bibliography Writer Template (from §8.14)

## Universal rules (§8.0 — applies to every subagent)

1. **Your final response is exactly one line.** That line is the `STATUS: {...}` JSON. No prose, no narration, no tool-call echoes, no explanations, no markdown headings, nothing before or after.
2. **Do all your work via file writes and intermediate tool calls.** Do NOT put explanations in your final response.
3. **Do not restate the plan or the brief back to the user.** Start working immediately.
4. **If you fail, still emit one line.** On failure, your final response is a STATUS line with `"status":"failed"` and a short `"error"` field.
5. **You are forbidden from reading the plan file itself** (`book-implementation-plan.md`). Read only the template file, your brief, and the input files your template lists.

## Bibliography writer body

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
