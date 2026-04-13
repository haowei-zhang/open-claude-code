# Diagrams Audit Report — Chapter 27

## Diagrams Found

### Diagram 1: Sequence Diagram (Extraction Lifecycle)
- **Type**: sequenceDiagram
- **Location**: Lines 99-123 of chapter
- **Validation**:
  - First non-empty line is `sequenceDiagram`: VALID
  - Balanced brackets: VALID
  - Edge operators (`->>`, `-->>`): VALID for sequenceDiagram
  - No Unicode arrows or smart quotes: VALID
  - Node count: 5 participants (QL, PS, SM, FA, MF): >= 3, NOT TRIVIAL
- **Useful**: TRUE

### Diagram 2: Flowchart (Section Size Enforcement Flow)
- **Type**: flowchart TD
- **Location**: Lines 361-376 of chapter
- **Validation**:
  - First non-empty line is `flowchart TD`: VALID
  - Balanced brackets: VALID
  - Edge operators (`-->`, `-->|yes|`, `-->|no|`): VALID for flowchart
  - No Unicode arrows or smart quotes: VALID
  - Node count: 14 nodes (A through N): >= 3, NOT TRIVIAL
- **Useful**: TRUE

## Required Diagrams (from brief)

1. (a) sequenceDiagram of memory extraction after a session — PRESENT (Diagram 1)
2. (b) flowchart of memory promotion session -> memdir — NOT PRESENT

The chapter has a flowchart about section size enforcement, but the brief requires a flowchart of "memory promotion session -> memdir". The existing flowchart covers a different topic (size enforcement and truncation), not the promotion pipeline from session memory to memdir.

## Summary

- Total diagrams: 2
- Invalid diagrams: 0
- Trivial diagrams: 0
- Type breakdown: sequenceDiagram: 1, flowchart: 1
- Missing required diagram: 1 (memory promotion session -> memdir)

## Verdict: revise (1 required diagram missing)
