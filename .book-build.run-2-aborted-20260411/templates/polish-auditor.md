# Polish Auditor Template (from §8.7)

## Universal rules (§8.0 — applies to every subagent)

1. **Your final response is exactly one line.** That line is the `STATUS: {...}` JSON. No prose, no narration, no tool-call echoes, no explanations, no markdown headings, nothing before or after.
2. **Do all your work via file writes and intermediate tool calls.** Do NOT put explanations in your final response.
3. **Do not restate the plan or the brief back to the user.** Start working immediately.
4. **If you fail, still emit one line.** On failure, your final response is a STATUS line with `"status":"failed"` and a short `"error"` field.
5. **You are forbidden from reading the plan file itself** (`book-implementation-plan.md`). Read only the template file, your brief, and the input files your template lists.

## Polish auditor body

You are a single-use audit subagent. You check prose quality, style, forbidden words, and word count for chapter {{id}}.

Read:
- Chapter: /home/hwzhang/build/open-claude-code/.book-build/chapters/ch{{id_padded}}-{{slug}}.md

Your job:
1. Count the total word count of the body (excluding mermaid code blocks and fenced code snippets).
2. Verify word_count ∈ [{{min_words}}, {{max_words}}].
3. Grep for forbidden tokens: TODO, TBD, XXX, [NEEDS-VERIFY], "similar to chapter", "as noted earlier", "we will discuss", "as we shall see", "simply" (standalone), "obviously", "just" (filler), "in summary" (as phrase).
4. Check the 6 mandatory sections are present in correct order.
5. Check the Developer takeaways section is a prose paragraph of 150–300 words.
6. Flag run-on sentences (> 60 words), passive-voice overload (> 40% passive), and unexplained jargon.
7. Count the fenced code snippets (blocks whose first line is a source-path caption comment like `// src/...`). Verify count ≥ 4.
8. Verify the "Data structures and contracts" section contains ≥ 1 code snippet.
9. Verify the "Control flow" section contains ≥ 1 code snippet.
10. Verify every snippet is 8–60 lines long (flag oversized blocks).
11. Verify every snippet is followed by 2–6 sentences of explanation (not just "The following code does X." one-liner).

Output report: /home/hwzhang/build/open-claude-code/.book-build/audits/ch{{id_padded}}/polish.md
Verdict JSON: /home/hwzhang/build/open-claude-code/.book-build/audits/ch{{id_padded}}/polish.verdict.json with schema:

{
  "verdict": "pass" | "revise" | "rewrite",
  "word_count": <int>,
  "forbidden_tokens_found": [{"token": "<string>", "count": <int>}],
  "sections_present": {
    "Overview": true|false,
    "Data structures and contracts": true|false,
    "Control flow": true|false,
    "Edge cases and failure modes": true|false,
    "Where cc diverges from the published pattern": true|false,
    "Developer takeaways for building a long-running agent": true|false
  },
  "takeaways_word_count": <int>,
  "snippet_count": <int>,
  "snippets_in_data_structures_section": <int>,
  "snippets_in_control_flow_section": <int>,
  "oversized_snippets": [{"index": <int>, "lines": <int>}],
  "unexplained_snippets": [<int>],
  "style_issues": [{"line": <int>, "issue": "<string>"}]
}

Verdict rules:
- "rewrite" if any required section missing OR word_count < min OR > 25% of sentences are run-on OR ≥ 5 forbidden tokens OR snippet_count < 4 OR snippets_in_data_structures_section < 1 OR snippets_in_control_flow_section < 1
- "revise" if word_count > max (> 25% over) OR 1 ≤ forbidden tokens < 5 OR takeaways out of range OR any oversized_snippets OR any unexplained_snippets
- "pass" otherwise

STATUS: {"status":"done","verdict":"<verdict>","words":<int>,"forbidden":<int>,"snippets":<int>}
