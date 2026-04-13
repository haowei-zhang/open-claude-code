# Diagrams Audit: Chapter 9 — System Prompts and Prompt Assembly

## Diagram Extraction

### Diagram 1: Prompt Assembly Layers (flowchart)
```mermaid
flowchart TD
    A[getSystemPrompt called] --> B{Simple mode?}
    B -->|Yes| C[Return minimal prompt]
    B -->|No| D[Load skillToolCommands, outputStyleConfig, envInfo]
    D --> E{Proactive mode?}
    E -->|Yes| F[Return proactive variant]
    E -->|No| G[Build static sections]
    G --> H[Intro section - identity and role]
    H --> I[System section - environment facts]
    I --> J[Doing tasks section - work guidelines]
    J --> K[Actions section - available operations]
    K --> L[Using tools section - tool usage rules]
    L --> M[Tone and style section - communication norms]
    M --> N[Output efficiency section - brevity guidelines]
    N --> O[Insert DYNAMIC_BOUNDARY marker]
    O --> P[Resolve dynamic sections via registry]
    P --> Q[session_guidance - per-session instructions]
    Q --> R[memory - CLAUDE.md files]
    R --> S[env_info - environment variables]
    S --> T[language / output_style - user preferences]
    T --> U[mcp_instructions - MCP server guidance]
    U --> V[scratchpad / frc / summarize - feature-specific]
    V --> W[token_budget / brief sections]
    W --> X[Return string array]
```
- Type: flowchart — valid
- Nodes: 24 (A through X) — non-trivial
- Edge operators: `-->` — valid for flowchart
- Balanced brackets: yes
- No Unicode arrows or smart quotes
- **Verdict: valid, useful**

### Diagram 2: CLAUDE.md Loading Hierarchy (flowchart BT)
```mermaid
flowchart BT
    subgraph "Priority (low to high)"
        A[Managed: /etc/claude-code/CLAUDE.md]
        B[User: ~/.claude/CLAUDE.md]
        C[Project Root: CLAUDE.md]
        D[Project .claude: .claude/CLAUDE.md]
        E[Rules: .claude/rules/*.md]
        F[Local: CLAUDE.local.md]
        G[AutoMem: MEMORY.md]
        H[TeamMem]
    end
    A --> B --> C --> D --> E --> F --> G --> H
    style A fill:#f9f,stroke:#333
    style H fill:#9f9,stroke:#333
```
- Type: flowchart — valid
- Nodes: 8 (A through H) — non-trivial
- Edge operators: `-->` — valid for flowchart
- Balanced brackets: yes
- No Unicode arrows or smart quotes
- **Verdict: valid, useful**

## Summary

- Diagram count: 2
- Required diagrams: 2 (brief requires "(a) flowchart of prompt assembly layers" and "(b) classDiagram of prompt fragment sources")
- Invalid indices: none
- Trivial indices: none
- Type breakdown: flowchart: 2, sequenceDiagram: 0, stateDiagram-v2: 0, classDiagram: 0, erDiagram: 0

## Note

The brief requires a "(b) classDiagram of prompt fragment sources" but the chapter provides a flowchart instead. The flowchart effectively shows the same information (the loading hierarchy) but in a different diagram type than specified. This is a minor mismatch — the content is covered but the diagram type differs.
