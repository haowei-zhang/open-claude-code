# Diagrams Audit: Chapter 30 — AppState: The Redux-like Store

## Required Diagrams

The brief requires:
- (a) classDiagram of store shape
- (b) sequenceDiagram of a state change propagating to subscribers

## Found Diagrams

### Diagram 1: classDiagram (line 165)
```
classDiagram
    class Store_T_ { ... }
    class AppState { ... }
    class BootstrapState { ... }
    class AppStateStore { ... }
    AppStateStore --> AppState : holds
    Store_T_ <|-- AppStateStore : implements
    BootstrapState : accessed via getters/setters
```
- Type: classDiagram — valid
- Nodes: 4 classes — adequate (>= 3)
- Brackets: balanced
- Edge operators: `-->`, `<|--` — valid for classDiagram
- Matches required diagram (a)

### Diagram 2: sequenceDiagram (line 368)
```
sequenceDiagram
    participant Caller as setAppState caller
    participant Store as Store<AppState>
    participant OnChange as onChangeAppState
    participant CCR as CCR / SDK
    participant React as useSyncExternalStore
    participant Config as GlobalConfig / Settings
    ... (messages with ->> and -->>)
```
- Type: sequenceDiagram — valid
- Participants: 6 — adequate (>= 3)
- Brackets: balanced
- Edge operators: `->>`, `-->>`, `--` — valid for sequenceDiagram
- Matches required diagram (b)

## Verdict

Both required diagrams present. Both syntactically valid with adequate complexity. Pass.
