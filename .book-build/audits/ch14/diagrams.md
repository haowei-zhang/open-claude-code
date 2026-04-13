# Diagrams Audit - Chapter 14: The Bash Tool, Classifiers, and Sandboxing

## Extracted Mermaid Diagrams

### Diagram 1: Permission evaluation pipeline (flowchart)
```mermaid
flowchart TD
    A[Model requests Bash command] --> B{Parse command}
    B --> C{checkReadOnlyConstraints}
    C -->|allow| D[Auto-approve, execute]
    C -->|passthrough| E{checkPermissionMode}
    E -->|allow| D
    E -->|passthrough| F{getDestructiveCommandWarning}
    F --> G[Show warning in permission dialog]
    G --> H{shouldUseSandbox}
    H -->|yes| I[Execute in sandbox]
    H -->|no| J[Execute on host]
    I --> K[Return result]
    J --> K
    D --> K
```
- Type: flowchart - VALID
- Nodes: 11 (A through K) - NOT trivial
- Edge operators: `-->` - VALID for flowchart
- Balanced brackets: YES
- No Unicode arrows or smart quotes: YES

### Diagram 2: State machine (stateDiagram-v2)
```mermaid
stateDiagram-v2
    [*] --> ParseCommand
    ParseCommand --> ReadOnlyCheck
    ReadOnlyCheck --> AutoApprove: is read-only
    ReadOnlyCheck --> ModeCheck: not read-only
    ModeCheck --> AutoApprove: acceptEdits + filesystem cmd
    ModeCheck --> SecurityCheck: passthrough
    SecurityCheck --> DestructiveWarning: dangerous patterns
    SecurityCheck --> SandboxDecision: no dangerous patterns
    DestructiveWarning --> SandboxDecision
    SandboxDecision --> SandboxedExecution: sandbox enabled
    SandboxDecision --> HostExecution: sandbox disabled
    SandboxedExecution --> [*]
    HostExecution --> [*]
    AutoApprove --> [*]
```
- Type: stateDiagram-v2 - VALID
- Nodes: 8 (ParseCommand, ReadOnlyCheck, AutoApprove, ModeCheck, SecurityCheck, DestructiveWarning, SandboxDecision, SandboxedExecution, HostExecution) - NOT trivial
- Edge operators: `-->` - VALID for stateDiagram-v2
- Balanced brackets: YES
- No Unicode arrows or smart quotes: YES

## Required Diagrams from Brief
1. (a) flowchart of a Bash command's risk classification - FOUND (Diagram 1)
2. (b) classDiagram of the classifier pipeline - NOT FOUND
3. (c) stateDiagram-v2 for sandbox / permission decisions - FOUND (Diagram 2)

## Summary
- Total diagrams: 2
- Minimum required: 2
- Brief required: 3
- Invalid: 0
- Trivial: 0
- Missing mandated diagram: classDiagram of the classifier pipeline (b)
