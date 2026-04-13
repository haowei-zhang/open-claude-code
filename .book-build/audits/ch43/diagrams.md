# Diagrams Audit: Chapter 43

## Verdict: revise

## Diagram Inventory

5 mermaid diagrams found in the chapter:

### Diagram 1: sequenceDiagram (L155)
- Type: sequenceDiagram (valid)
- Topic: Keystroke to model submission
- Nodes: 7 (Terminal, Ink, GlobalKeybindings, PromptInput, Typeahead, REPL, Query)
- Valid: Yes
- Trivial: No (7 actors)
- Required match: (b) sequenceDiagram of a prompt from keystroke to model send -- MATCHED

### Diagram 2: flowchart TD (L199)
- Type: flowchart TD (valid)
- Topic: Typeahead suggestion pipeline
- Nodes: 13 (A through M)
- Valid: Yes
- Trivial: No
- Required match: No specific flowchart required by brief

### Diagram 3: flowchart TD (L233)
- Type: flowchart TD (valid)
- Topic: Typeahead suggestion pipeline (continued)
- Nodes: 11 (A through K)
- Valid: Yes
- Trivial: No
- **DUPLICATE**: This is a near-duplicate of Diagram 2 with slight differences (no Slack channel branch, fewer nodes)

### Diagram 4: stateDiagram-v2 (L271)
- Type: stateDiagram-v2 (valid)
- Topic: Voice capture lifecycle
- Nodes: 4 (Idle, Recording, Processing, plus [*])
- Valid: Yes
- Trivial: No
- Required match: (c) stateDiagram-v2 of voice capture -- MATCHED

### Diagram 5: stateDiagram-v2 (L340)
- Type: stateDiagram-v2 (valid)
- Topic: Voice capture lifecycle (duplicate)
- **EXACT DUPLICATE of Diagram 4** (same hash)

## Required Diagrams

Brief requires:
- (a) classDiagram of REPL components -- **MISSING**
- (b) sequenceDiagram of a prompt from keystroke to model send -- Found (Diagram 1)
- (c) stateDiagram-v2 of voice capture -- Found (Diagram 4, but also duplicated as Diagram 5)

## Issues

1. **Missing required diagram**: classDiagram of REPL components is not present.
2. **Duplicate diagrams**: Diagram 5 is an exact duplicate of Diagram 4. Diagram 3 is a near-duplicate of Diagram 2.
3. Diagram count is 5, which exceeds the minimum of 2, but only 3 are unique.

## Summary
- diagram_count: 5 (3 unique)
- invalid_indices: []
- trivial_indices: []
- type_breakdown: {"flowchart": 2, "sequenceDiagram": 1, "stateDiagram-v2": 2, "classDiagram": 0, "erDiagram": 0}
