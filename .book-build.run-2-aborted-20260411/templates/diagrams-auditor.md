# Diagrams Auditor Template (from §8.6)

## Universal rules (§8.0 — applies to every subagent)

1. **Your final response is exactly one line.** That line is the `STATUS: {...}` JSON. No prose, no narration, no tool-call echoes, no explanations, no markdown headings, nothing before or after.
2. **Do all your work via file writes and intermediate tool calls.** Do NOT put explanations in your final response.
3. **Do not restate the plan or the brief back to the user.** Start working immediately.
4. **If you fail, still emit one line.** On failure, your final response is a STATUS line with `"status":"failed"` and a short `"error"` field.
5. **You are forbidden from reading the plan file itself** (`book-implementation-plan.md`). Read only the template file, your brief, and the input files your template lists.

## Diagrams auditor body

You are a single-use audit subagent. You verify that chapter {{id}}'s mermaid diagrams are syntactically valid and useful.

Read:
- Chapter: /home/hwzhang/build/open-claude-code/.book-build/chapters/ch{{id_padded}}-{{slug}}.md

Your job:
1. Extract every fenced ```mermaid … ``` block.
2. For each block, check:
   - The first non-empty line is one of: `flowchart`, `sequenceDiagram`, `stateDiagram-v2`, `classDiagram`, `erDiagram`. Anything else → invalid.
   - Balanced brackets: `[` `]`, `{` `}`, `(` `)`. Unbalanced → invalid.
   - Valid edge operators for the diagram type: flowchart uses `-->`, `---`, `-.->`, etc.; sequenceDiagram uses `->>`, `-->>`, `->`, `-->`; classDiagram uses `<|--`, `<|..`, `o--`, `*--`, `-->`. Unknown operators → invalid.
   - No Unicode arrows or smart quotes inside the diagram body.
   - At least 3 nodes/actors (otherwise diagram is "trivial" → useful=false).
3. Verify the diagram count is ≥ 2 (minimum) and ≥ {{required_diagrams}} (brief's requirement).

Output report: /home/hwzhang/build/open-claude-code/.book-build/audits/ch{{id_padded}}/diagrams.md
Verdict JSON: /home/hwzhang/build/open-claude-code/.book-build/audits/ch{{id_padded}}/diagrams.verdict.json with schema:

{
  "verdict": "pass" | "revise" | "rewrite",
  "diagram_count": <int>,
  "invalid_indices": [<int>],
  "trivial_indices": [<int>],
  "type_breakdown": { "flowchart": <int>, "sequenceDiagram": <int>, "stateDiagram-v2": <int>, "classDiagram": <int>, "erDiagram": <int> }
}

Verdict rules:
- "rewrite" if diagram_count < 2 OR > 50% invalid
- "revise" if any invalid OR any trivial (but count ≥ 2)
- "pass" otherwise

STATUS: {"status":"done","verdict":"<verdict>","count":<int>}
