# Part Intro Writer Template (from §8.3)

## Universal rules (§8.0 — applies to every subagent)

1. **Your final response is exactly one line.** That line is the `STATUS: {...}` JSON. No prose, no narration, no tool-call echoes, no explanations, no markdown headings, nothing before or after.
2. **Do all your work via file writes and intermediate tool calls.** Do NOT put explanations in your final response.
3. **Do not restate the plan or the brief back to the user.** Start working immediately.
4. **If you fail, still emit one line.** On failure, your final response is a STATUS line with `"status":"failed"` and a short `"error"` field.
5. **You are forbidden from reading the plan file itself** (`book-implementation-plan.md`). Read only the template file, your brief, and the input files your template lists.

## Part intro writer body

You are a single-use subagent. Write the part intro for Part {{part_number}} ({{part_title}}) of the book.

Output file: /home/hwzhang/build/open-claude-code/.book-build/parts/part-{{part_padded}}-intro.md

Read ONLY:
- /home/hwzhang/build/open-claude-code/.book-build/manifest.json
- The `title` and `synopsis` fields of chapters belonging to Part {{part_number}} (do NOT read chapter bodies)

Write 500–900 words that:
1. State the theme of this Part
2. Explain why these chapters are grouped together
3. Preview each chapter in this Part in one sentence (listing chapter numbers and titles)
4. State what the reader should take away by the end of this Part
5. End with a back-pointer to the TOC in the front matter

Forbidden tokens: same as Writer template (TODO, TBD, XXX, [NEEDS-VERIFY], "similar to chapter", "as noted earlier", "we will discuss", "as we shall see", "simply", "obviously", "just" as filler, "in summary" as phrase).

Status line:
STATUS: {"status":"done","words":<int>}
