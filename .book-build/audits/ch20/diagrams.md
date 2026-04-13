# Diagrams Audit: Chapter 20

## Diagram 1: classDiagram (line 159-200)

```mermaid
classDiagram
    class BaseAgentDefinition {
        +agentType: string
        +whenToUse: string
        +tools: string[]
        +disallowedTools: string[]
        +skills: string[]
        +mcpServers: AgentMcpServerSpec[]
        +hooks: HooksSettings
        +model: string
        +effort: EffortValue
        +permissionMode: PermissionMode
        +maxTurns: number
        +requiredMcpServers: string[]
        +background: boolean
        +memory: AgentMemoryScope
        +isolation: worktree | remote
        +omitClaudeMd: boolean
    }
    class BuiltInAgentDefinition {
        +source: built-in
        +baseDir: built-in
        +callback: function
        +getSystemPrompt(params): string
    }
    class CustomAgentDefinition {
        +source: SettingSource
        +getSystemPrompt(): string
        +filename: string
        +baseDir: string
    }
    class PluginAgentDefinition {
        +source: plugin
        +plugin: string
        +getSystemPrompt(): string
        +filename: string
    }
    BaseAgentDefinition <|-- BuiltInAgentDefinition
    BaseAgentDefinition <|-- CustomAgentDefinition
    BaseAgentDefinition <|-- PluginAgentDefinition
```

- Type: classDiagram (valid)
- Nodes: 4 classes (>= 3, not trivial)
- Brackets: Balanced
- Edge operators: `<|--` (valid for classDiagram inheritance)
- No Unicode arrows or smart quotes
- Verdict: VALID, USEFUL

## Diagram 2: flowchart (line 272-283)

```mermaid
flowchart TD
    A[Start agent discovery] --> B[Load built-in agents]
    B --> C[Load plugin agents]
    C --> D[Load user agents from ~/.claude/agents/]
    D --> E[Load project agents from .claude/agents/]
    E --> F[Load flag-based agents]
    F --> G[Load managed policy agents]
    G --> H[Apply precedence: later source overrides earlier]
    H --> I[Filter by requiredMcpServers]
    I --> J[Return activeAgents + allAgents]
```

- Type: flowchart (valid)
- Nodes: 10 nodes (>= 3, not trivial)
- Brackets: Balanced `[]` pairs
- Edge operators: `-->` (valid for flowchart)
- No Unicode arrows or smart quotes
- Verdict: VALID, USEFUL

## Required Diagrams from Brief

Brief requires:
1. (a) flowchart of agent discovery across directories - PROVIDED (flowchart TD)
2. (b) classDiagram of AgentDefinition shape - PROVIDED (classDiagram)

## Summary

- Total diagrams: 2
- Required: 2
- Invalid: 0
- Trivial: 0
- Type breakdown: classDiagram=1, flowchart=1

## Verdict: pass
