You are a single-use subagent. Write the front matter file for the 57-chapter book "Deep Research and Development Guide of CC Source Code".

Output file: /home/hwzhang/build/open-claude-code/.book-build/frontmatter.md

Read:
- /home/hwzhang/build/open-claude-code/.book-build/manifest.json (full)
- First 20 lines only of each chapter file under .book-build/chapters/ (head, via offset/limit)

Produce (Markdown):
# Deep Research and Development Guide of CC Source Code
A byline line with the repo SHA from .book-build/repo-sha.txt.

## Preface
800–1,200 words. Explain the origin of the book (leaked cc codebase + HER report), why it exists, the intended audience (engineers building long-running agent harnesses), and the reading paths available (linear vs subsystem-focused vs pattern-focused).

## How to Read This Book
Explain citation format (src/path/file.ts:Lnnn), diagram conventions (the 5 allowed mermaid types), chapter structure (the 6 mandatory sections), and cross-reference notation.

## Table of Contents
Autogenerate from manifest.json. Structure:
- **Part I. Foundations and Framing**
  - Chapter 1. Why This Book Exists: The Harness Engineering Moment
  - ...
- **Part II. Entry, Bootstrap, and the Runtime Spine**
  - ...
Include all 57 chapters, 10 parts, 3 appendices.

## Forbidden Tokens
None in your output.

Status line (last line):
STATUS: {"status":"done","words":<int>}
# Universal rules (apply to every subagent)

1. **Your final response is exactly one line.** That line is the `STATUS: {...}` JSON. No prose, no narration, no tool-call echoes, no explanations, no markdown headings, nothing before or after. The parent receives this line as your sole output and any extra bytes bloat its context.
2. **Do all your work via file writes and intermediate tool calls.** If you want to explain what you did, write it to a file under `.book-build/` (e.g., the audit prose reports go to `.md` files, chapter bodies go to chapter files). Do NOT put explanations in your final response.
