# Diagrams Audit: Chapter 19 — Sync vs Fork vs Remote: The Execution Modes

## Diagram Inventory

### Diagram 1: Sync mode execution (line ~169)
```mermaid
sequenceDiagram
    participant Parent as Parent Query Loop
    participant AgentTool as AgentTool.call()
    participant RunAgent as runAgent()
    participant SubAgent as Subagent Query Loop

    Parent->>AgentTool: Invoke with subagent_type
    AgentTool->>RunAgent: Create subagent context
    RunAgent->>RunAgent: Initialize MCP servers
    RunAgent->>RunAgent: Clone file state cache
    RunAgent->>SubAgent: query(promptMessages)
    SubAgent->>SubAgent: Execute turns
    SubAgent->>RunAgent: Return messages + usage
    RunAgent->>RunAgent: Cleanup MCP connections
    RunAgent->>AgentTool: Return agent result
    AgentTool->>Parent: Return completed status
```
- Type: sequenceDiagram — valid
- Balanced brackets: YES
- Valid edge operators: `->>` — valid for sequenceDiagram
- No Unicode arrows or smart quotes
- Nodes/actors: 4 (Parent, AgentTool, RunAgent, SubAgent) — non-trivial
- Verdict: **valid, useful**

### Diagram 2: Fork mode execution (line ~273)
```mermaid
sequenceDiagram
    participant Parent as Parent Agent
    participant ForkSub as forkSubagent.ts
    participant Child as Forked Child Process

    Parent->>ForkSub: Omit subagent_type (fork gate on)
    ForkSub->>ForkSub: buildForkedMessages(directive)
    ForkSub->>ForkSub: Clone assistant message
    ForkSub->>ForkSub: Insert placeholder tool_results
    ForkSub->>ForkSub: Append child directive
    ForkSub->>Child: Bun.fork() with forked messages
    Child->>Child: Execute directive
    Child->>Child: Report: Scope/Result/Files
    Child->>Parent: Return fork result
```
- Type: sequenceDiagram — valid
- Balanced brackets: YES
- Valid edge operators: `->>` — valid for sequenceDiagram
- No Unicode arrows or smart quotes
- Nodes/actors: 3 (Parent, ForkSub, Child) — non-trivial
- Verdict: **valid, useful**

### Diagram 3: Remote mode execution (line ~331)
```mermaid
sequenceDiagram
    participant Parent as Parent Agent
    participant Remote as CCR Cloud Environment
    participant Poller as Local Poller

    Parent->>Remote: teleportToRemote(prompt)
    Remote->>Remote: Create cloud session
    Remote-->>Parent: sessionUrl + taskId
    Parent->>Poller: Register RemoteAgentTaskState
    
    loop Poll cycle
        Poller->>Remote: fetchSession(sessionId)
        Remote-->>Poller: SDK events + progress
        Poller->>Poller: Append events to task output
    end
    
    Remote->>Poller: Task completed/failed
    Poller->>Parent: <task-notification>
```
- Type: sequenceDiagram — valid
- Balanced brackets: YES
- Valid edge operators: `->>`, `-->>` — valid for sequenceDiagram
- No Unicode arrows or smart quotes
- Nodes/actors: 3 (Parent, Remote, Poller) — non-trivial
- Verdict: **valid, useful**

## Brief Requirements

Required diagrams from brief:
- (a) sequenceDiagram for sync mode — **FOUND** (Diagram 1)
- (b) sequenceDiagram for fork mode — **FOUND** (Diagram 2)
- (c) sequenceDiagram for remote mode — **FOUND** (Diagram 3)

## Summary

- Diagram count: 3
- Minimum required: 2 (general), 3 (brief)
- All required diagram types present
- All diagrams syntactically valid and non-trivial
- Type breakdown: sequenceDiagram: 3
