# Diagrams Audit: Chapter 6 — `main.tsx` and the Command Router

## Diagram Count: 2

### Diagram 1: Command Routing Flowchart
- **Type**: flowchart TD
- **Nodes**: 15 (A through O)
- **Edges**: 14
- **Validity**: VALID
  - First non-empty line is `flowchart TD`
  - Balanced brackets
  - Valid edge operators (`-->`)
  - No Unicode arrows or smart quotes
  - ≥3 nodes (15 nodes)
- **Useful**: YES — illustrates the main routing decisions in main.tsx
- **Required**: (a) flowchart of command routing decisions — MATCHED

### Diagram 2: Headless vs Interactive Sequence Diagram
- **Type**: sequenceDiagram
- **Participants**: 5 (User, main.tsx, Commander, renderAndRun, Headless)
- **Messages**: 10
- **Validity**: VALID
  - First non-empty line is `sequenceDiagram`
  - Valid operators (`->>`)
  - No Unicode arrows
  - ≥3 participants
- **Useful**: YES — shows the bifurcation between headless and interactive modes
- **Required**: (b) sequenceDiagram headless vs interactive launch — MATCHED

## Type Breakdown
- flowchart: 1
- sequenceDiagram: 1
- stateDiagram-v2: 0
- classDiagram: 0
- erDiagram: 0
