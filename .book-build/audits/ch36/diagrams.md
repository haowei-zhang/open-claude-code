# Diagrams Audit: Chapter 36 - The Hook Schema and Lifecycle Events

## Diagram Extraction

### Diagram 1: Hook discovery flowchart (line ~213)
```
flowchart TD
    A[getAllHooks] --> B{allowManagedHooksOnly?}
    B -->|yes| C[Skip user/project/local hooks]
    B -->|no| D[Collect from userSettings]
    D --> E[Collect from projectSettings]
    E --> F[Collect from localSettings]
    F --> G[Collect session hooks]
    G --> H[Collect plugin hooks]
    H --> I[Collect built-in hooks]
    C --> G
    I --> J[Return IndividualHookConfig array]
```
- Type: flowchart - VALID
- Brackets: balanced
- Edge operators: `-->` - valid for flowchart
- No Unicode arrows or smart quotes
- Nodes: A, B, C, D, E, F, G, H, I, J = 10 nodes - NOT trivial
- Useful: YES

### Diagram 2: Hook schema classDiagram (line ~279)
```
classDiagram
    class HookEvent { +string summary +string description +MatcherMetadata matcherMetadata }
    class MatcherMetadata { +string fieldToMatch +string[] values }
    class HookCommand { +string type +string if +number timeout +string statusMessage +boolean once }
    class BashCommandHook { +string command +string shell +boolean async +boolean asyncRewake }
    class PromptHook { +string prompt +string model }
    class HttpHook { +string url +Record headers +string[] allowedEnvVars }
    class AgentHook { +string prompt +string model }
    HookEvent --> MatcherMetadata
    HookCommand <|-- BashCommandHook
    HookCommand <|-- PromptHook
    HookCommand <|-- HttpHook
    HookCommand <|-- AgentHook
```
- Type: classDiagram - VALID
- Brackets: balanced
- Edge operators: `-->`, `<|--` - valid for classDiagram
- No Unicode arrows or smart quotes
- Nodes: HookEvent, MatcherMetadata, HookCommand, BashCommandHook, PromptHook, HttpHook, AgentHook = 7 classes - NOT trivial
- Useful: YES

## Required Diagrams from Brief

1. (a) classDiagram of hook schema - FOUND (Diagram 2)
2. (b) erDiagram of all events with triggers - NOT FOUND. No erDiagram present.
3. (c) flowchart of exit-code handling - NOT FOUND. No flowchart for exit-code handling.

## Summary

- Diagram count: 2 (minimum 2 met)
- Required diagrams: 3, found: 1 matching (classDiagram of hook schema). Missing: erDiagram of events with triggers, flowchart of exit-code handling.
- Invalid indices: []
- Trivial indices: []
