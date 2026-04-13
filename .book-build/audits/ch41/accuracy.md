# Accuracy Audit for Chapter 41

## Summary
Chapter 41 covers MCP tools (MCPTool, ListMcpResourcesTool, ReadMcpResourceTool) and their passthrough permission model and deferred loading. The chapter heavily cites `src/services/mcp/client.ts` which is not listed in the brief's source_files but is the primary implementation file.

## Issues Found

### 1. Inaccurate claim: "shouldDefer: true" on MCPTool (line 7)
The chapter states: "All three MCP tool wrappers set `shouldDefer: true`". However, `MCPTool` in `src/tools/MCPTool/MCPTool.ts` does NOT have `shouldDefer: true` in its `buildTool()` definition. Instead, MCP tools are deferred through the `isDeferredTool()` function in `src/tools/ToolSearchTool/prompt.ts:L62-68`, which checks `tool.isMcp === true`. Only `ListMcpResourcesTool` and `ReadMcpResourceTool` explicitly set `shouldDefer: true`.

### 2. Snippet drift: MCPTool.ts:L27-L55 (line 16)
The cited range L27-L55 ends at `call()` returning `{data: ''}`, but the chapter snippet extends beyond with `// ...` then shows `checkPermissions()` which is at lines 56-61. The line range is misleading.

### 3. Snippet drift: client.ts:L1767-L1799 (line 58)
Chapter snippet omits some comments present in the actual source (e.g., the "In skip-prefix mode" comment is present but some inline comments are trimmed). Minor drift in formatting.

### 4. Snippet range error: client.ts:L1249-L1365 (line 282)
The cited range L1249-L1365 spans 117 lines, but the chapter only shows the `isTerminalConnectionError` function (lines 1249-1263, about 15 lines). The range is vastly wider than the snippet shown.

### 5. Snippet drift: ReadMcpResourceTool.ts:L106-L139 (line 377)
Chapter snippet shows `getBinaryBlobSavedMessage(persisted.filepath, c.mimeType, persisted.size, ...)` with `...` trim marker, but the actual code has a 4th argument `[Resource from ${serverName} at ${c.uri}]`. The trim is acceptable but the argument is functionally significant.

### 6. Snippet range error: client.ts:L1429-L1570 (line 460)
Cited range spans 142 lines but chapter shows a heavily simplified ~24-line version. The actual code has much more detail in the signal escalation logic.

### 7. Uncited source: src/tools/McpAuthTool/McpAuthTool.ts
This file is listed in the brief's source_files but while the chapter discusses McpAuthTool, it does not include any code snippet from this file.

## Verification Results
- 15 snippets total, 4 clearly verbatim, 6 with drift, 5 heavily trimmed but accurate in substance
- All cited files exist
- Main factual error: the shouldDefer claim for MCPTool
- No hallucinated snippets detected
