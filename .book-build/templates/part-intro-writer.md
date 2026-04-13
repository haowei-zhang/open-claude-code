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

Forbidden tokens: same as Writer template.

Status line:
STATUS: {"status":"done","words":<int>}
# Universal rules (apply to every subagent)

1. **Your final response is exactly one line.** That line is the `STATUS: {...}` JSON. No prose, no narration, no tool-call echoes, no explanations, no markdown headings, nothing before or after. The parent receives this line as your sole output and any extra bytes bloat its context.
2. **Do all your work via file writes and intermediate tool calls.** If you want to explain what you did, write it to a file under `.book-build/` (e.g., the audit prose reports go to `.md` files, chapter bodies go to chapter files). Do NOT put explanations in your final response.
