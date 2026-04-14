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
