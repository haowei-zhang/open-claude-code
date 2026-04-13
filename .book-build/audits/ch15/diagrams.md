# Diagrams Audit: Chapter 15 — Search, LSP, and Code Analysis Tools

## Extracted mermaid diagrams

### Diagram 1: LSP Tool sequence diagram (line ~211)
```
sequenceDiagram
    participant Model
    participant LSPTool
    participant LSPManager
    participant LanguageServer

    Model->>LSPTool: goToDefinition /path/to/file.ts:42:10
    LSPTool->>LSPManager: getConnectedServer("typescript")
    LSPManager-->>LSPTool: server instance
    LSPTool->>LSPManager: waitForInitialization
    LSPManager->>LanguageServer: await initialization
    LanguageServer-->>LSPManager: initialized
    LSPTool->>LanguageServer: textDocument/definition
    LanguageServer-->>LSPTool: Location[]
    LSPTool->>LSPTool: formatGoToDefinitionResult
    LSPTool-->>Model: "Definition at /other/file.ts:L15:C5"
```

**Check:**
- Type: `sequenceDiagram` — VALID
- Brackets: balanced
- Edge operators: `->>`, `-->>` — VALID for sequenceDiagram
- No Unicode arrows or smart quotes
- Nodes/actors: 4 (Model, LSPTool, LSPManager, LanguageServer) — non-trivial
- **Verdict: VALID, USEFUL**

### Diagram 2: ToolSearch keyword search sequence diagram (line ~240)
```
sequenceDiagram
    participant Model
    participant ToolSearch
    participant DeferredTools
    participant FullToolSet

    Model->>ToolSearch: query="notebook edit"
    ToolSearch->>DeferredTools: filter isDeferredTool
    ToolSearch->>ToolSearch: parseToolName for each
    ToolSearch->>ToolSearch: getToolDescriptionMemoized
    ToolSearch->>ToolSearch: score by name/hint/description match
    ToolSearch-->>Model: {matches: ["NotebookEdit"], total_deferred_tools: 25}

    Model->>ToolSearch: query="select:NotebookEdit"
    ToolSearch->>DeferredTools: findByName("NotebookEdit")
    DeferredTools-->>ToolSearch: found
    ToolSearch-->>Model: {matches: ["NotebookEdit"], total_deferred_tools: 25}
```

**Check:**
- Type: `sequenceDiagram` — VALID
- Brackets: balanced
- Edge operators: `->>`, `-->>` — VALID for sequenceDiagram
- No Unicode arrows or smart quotes
- Nodes/actors: 4 (Model, ToolSearch, DeferredTools, FullToolSet) — non-trivial
- **Verdict: VALID, USEFUL**

### Diagram 3: ToolSearchTool class diagram (line ~280)
```
classDiagram
    class ToolSearchTool {
        +name: TOOL_SEARCH_TOOL_NAME
        +isReadOnly: true
        +isConcurrencySafe: true
        +call(input, context) Output
        +searchToolsWithKeywords(query, deferredTools, tools, maxResults)
    }

    class DeferredTool {
        +name: string
        +searchHint: string
        +prompt(context) string
        +isDeferred: true
    }

    class ParsedToolName {
        +parts: string[]
        +full: string
        +isMcp: boolean
    }

    ToolSearchTool --> DeferredTool : searches
    ToolSearchTool --> ParsedToolName : parses names
    DeferredTool --> ToolSearchTool : provides descriptions
```

**Check:**
- Type: `classDiagram` — VALID
- Brackets: balanced
- Edge operators: `-->` — VALID for classDiagram
- No Unicode arrows or smart quotes
- Nodes/actors: 3 classes — non-trivial
- **Verdict: VALID, USEFUL**

## Summary

- Diagram count: 3
- Required minimum: 2
- Required by brief: 2 (sequenceDiagram of ToolSearch -> deferred tool -> first call; classDiagram of LSP client lifecycle)
- Invalid indices: none
- Trivial indices: none
- Type breakdown: { "sequenceDiagram": 2, "classDiagram": 1 }

Note: The brief requires a "classDiagram of LSP client lifecycle" but the chapter provides a classDiagram of ToolSearchTool instead. The LSP client lifecycle is described in prose in the "Deep dive: the LSPTool server lifecycle" section but without a class diagram. This is a gap but not a diagram validity issue.
