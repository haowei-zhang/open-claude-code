# Gaps Audit Report: Chapter 32

## Brief Coverage
- Synopsis matches: Yes (covers permission modes and rule evaluation)
- All source files cited: Yes

## Diagram Requirements
Brief requires 3 diagrams: (a) stateDiagram-v2 of permission modes, (b) flowchart of rule evaluation, (c) classDiagram of rule types.

Found:
- (a) stateDiagram-v2 of permission pipeline — present (covers modes)
- (b) flowchart of rule evaluation — present
- (c) classDiagram of rule types — **missing**

The brief requires a classDiagram of rule types (PermissionRule, PermissionRuleSource, PermissionBehavior, PermissionRuleValue) but the chapter only has prose descriptions and code snippets showing these types, not a mermaid classDiagram.

## Minimum Counts
- Citations: 42 (minimum: 6) — pass
- Diagrams: 2 (minimum: 2) — pass
- Snippets: 9 (minimum: 4) — pass

## Top Source Files Without Snippets
None — all 3 primary source files (permissions.ts, PermissionMode.ts, PermissionRule.ts) have snippets.

Verdict: **revise** (1 missing mandated diagram: classDiagram of rule types)
