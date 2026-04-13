# Diagrams Audit: Chapter 01

## Diagram Inventory

### Diagram 1: "Prompt->Context->Harness Evolution" (line 65)
- Type: `flowchart TD`
- Nodes: 9 (A through I)
- Edges: 8 valid `-->` operators
- Bracket balance: OK
- No Unicode arrows or smart quotes
- Verdict: Valid, not trivial

### Diagram 2: "TerminalBench 2.0: Same Model, Different Harness" (line 111)
- Type: `flowchart LR`
- Nodes: 6 (A through F)
- Edges: 4 valid `-->` operators with labels
- Uses `subgraph` correctly
- Bracket balance: OK
- No Unicode arrows or smart quotes
- Verdict: Valid, not trivial

### Diagram 3: "Task Duration Doubling Timeline" (line 122)
- Type: `flowchart LR`
- Nodes: 7 (A through G)
- Edges: 6 valid `-->` operators
- Bracket balance: OK
- No Unicode arrows or smart quotes
- Verdict: Valid, not trivial

## Required Diagrams Check
- (a) flowchart model + harness = agent block diagram: Diagram 1 satisfies this
- (b) timeline flowchart METR task-duration doubling: Diagram 3 satisfies this

## Summary
All 3 diagrams are syntactically valid flowcharts with sufficient complexity. Both required diagrams are present.
