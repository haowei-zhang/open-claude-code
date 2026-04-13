# Diagrams Audit: Chapter 39

## Extracted Mermaid Diagrams

### Diagram 1: classDiagram (CommandBase hierarchy)
Location: Lines 299-330
```
classDiagram
    class CommandBase { ... }
    class PromptCommand { ... }
    class LocalCommand { ... }
    class LocalJSXCommand { ... }
    CommandBase <|-- PromptCommand
    CommandBase <|-- LocalCommand
    CommandBase <|-- LocalJSXCommand
```
- Type: classDiagram - VALID
- Balanced brackets: YES
- Valid edge operators for classDiagram: `<|--` - VALID
- No Unicode arrows: YES
- Node count: 4 (CommandBase, PromptCommand, LocalCommand, LocalJSXCommand) - ≥3, NOT trivial
- Verdict: PASS

### Diagram 2: sequenceDiagram (slash command dispatch)
Location: Lines 388-408
```
sequenceDiagram
    participant User as REPL Input
    participant PSP as parseSlashCommand
    participant CR as Command Registry
    participant SC as processSlashCommand
    participant Agent as runAgent
    User->>PSP: /commit -m "fix bug"
    PSP->>PSP: Strip /, split name + args
    PSP->>CR: findCommand("commit")
    CR-->>PSP: PromptCommand
    PSP->>SC: dispatch(command, args)
    SC->>SC: getMessagesForPromptSlashCommand
    alt context = fork
        SC->>Agent: runAgent(agentDef, promptMessages)
        Agent-->>SC: resultText
    else context = inline
        SC->>SC: Expand prompt into newMessages
        SC-->>User: Return messages to query loop
    end
```
- Type: sequenceDiagram - VALID
- Balanced brackets: YES
- Valid edge operators: `->>`, `-->>` - VALID
- No Unicode arrows: YES
- Node count: 5 participants - ≥3, NOT trivial
- Verdict: PASS

### Diagram 3: flowchart (command dispatch flow)
Location: Lines 410-426
```
flowchart
    A[User types /command] --> B{parseSlashCommand}
    B -->|No slash| C[Normal input]
    B -->|Has slash| D{findCommand}
    D -->|Not found| E[Unknown command error]
    D -->|Found| F{Command type?}
    F -->|prompt| G{context = fork?}
    G -->|yes| H[executeForkedSkill: runAgent]
    G -->|no| I[Expand prompt into messages]
    F -->|local| J[load + call: return text]
    F -->|local-jsx| K[load + call: render Ink UI]
    H --> L[Return forked result]
    I --> M[Send to model as user message]
    J --> N[Display text result]
    K --> O[Render interactive UI]
```
- Type: flowchart - VALID
- Balanced brackets: YES
- Valid edge operators: `-->` - VALID
- No Unicode arrows: YES
- Node count: 15 nodes - ≥3, NOT trivial
- Verdict: PASS

## Summary

- Diagram count: 3
- Required minimum: 2
- Brief requirement: 2 (classDiagram + sequenceDiagram)
- Invalid diagrams: 0
- Trivial diagrams: 0
- Type breakdown: classDiagram=1, sequenceDiagram=1, flowchart=1
