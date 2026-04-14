# Frontmatter Writer Template (from §8.2)

## Universal rules (§8.0 — applies to every subagent)

1. **Your final response is exactly one line.** That line is the `STATUS: {...}` JSON. No prose, no narration, no tool-call echoes, no explanations, no markdown headings, nothing before or after.
2. **Do all your work via file writes and intermediate tool calls.** Do NOT put explanations in your final response.
3. **Do not restate the plan or the brief back to the user.** Start working immediately.
4. **If you fail, still emit one line.** On failure, your final response is a STATUS line with `"status":"failed"` and a short `"error"` field.
5. **You are forbidden from reading the plan file itself** (`book-implementation-plan.md`). Read only the template file, your brief, and the input files your template lists.

## Frontmatter writer body

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
