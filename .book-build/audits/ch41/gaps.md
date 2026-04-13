# Gaps Audit for Chapter 41

## Summary
Checking what the chapter failed to cover that its brief required.

## Brief Requirements

### Synopsis Match
The brief synopsis: "MCPTool, ListMcpResourcesTool, ReadMcpResourceTool, McpAuthTool. Why MCP tools are deferred and how ToolSearch surfaces them."
The chapter covers all four tools and explains the deferral mechanism and ToolSearch integration. Synopsis is well matched.

### Source Files Coverage
Brief source files:
1. `src/tools/MCPTool/MCPTool.ts` - CITED (multiple snippets)
2. `src/tools/ListMcpResourcesTool/ListMcpResourcesTool.ts` - CITED (multiple snippets)
3. `src/tools/ListMcpResourcesTool/prompt.ts` - NOT CITED
4. `src/tools/ListMcpResourcesTool/UI.tsx` - NOT CITED
5. `src/tools/ReadMcpResourceTool/ReadMcpResourceTool.ts` - CITED (multiple snippets)
6. `src/tools/ReadMcpResourceTool/prompt.ts` - NOT CITED
7. `src/tools/ReadMcpResourceTool/UI.tsx` - NOT CITED
8. `src/tools/McpAuthTool/McpAuthTool.ts` - MENTIONED but no snippet

Note: `src/services/mcp/client.ts` is heavily used (10+ snippets) but not listed in the brief's source_files.

### Uncited Brief Files
- `src/tools/ListMcpResourcesTool/prompt.ts`
- `src/tools/ListMcpResourcesTool/UI.tsx`
- `src/tools/ReadMcpResourceTool/prompt.ts`
- `src/tools/ReadMcpResourceTool/UI.tsx`
- `src/tools/McpAuthTool/McpAuthTool.ts` (discussed but no snippet)

4 completely uncited brief files (prompt.ts and UI.tsx for List and Read tools). The prompt.ts and UI.tsx files are typically not central to the chapter's technical content (they contain tool descriptions and UI rendering), so these are minor gaps. However, McpAuthTool.ts is a significant gap since it's listed as a source file and McpAuthTool is an important tool.

### Mandated Diagrams
Brief requires:
- (a) sequenceDiagram of a deferred MCP tool first call - PRESENT (line 180)
- (b) classDiagram of MCP tool wrapper - PRESENT (line 304, shows transport class hierarchy which is the wrapper layer)

Both diagrams present.

### Minimum Counts
- Citations: 33 (>= 6 required) - PASS
- Diagrams: 2 (>= 2 required) - PASS
- Snippets: 15 (>= 4 required) - PASS

### Top Files Without Snippets
The top 3 most central source files:
1. `src/tools/MCPTool/MCPTool.ts` - HAS snippets
2. `src/tools/ReadMcpResourceTool/ReadMcpResourceTool.ts` - HAS snippets
3. `src/tools/ListMcpResourcesTool/ListMcpResourcesTool.ts` - HAS snippets

No top file without a snippet.

### Uncovered Topics
1. **McpAuthTool implementation details**: The chapter mentions McpAuthTool but doesn't show any code from `McpAuthTool.ts` or explain its internal mechanics (how it triggers the OAuth flow, how reconnection is initiated after auth).
2. **ToolSearchTool keyword matching for MCP tools**: The chapter mentions that ToolSearchTool catalogs deferred tools but doesn't explain how MCP-specific search hints are matched (the keyword scoring and matching algorithm).
3. **MCP tool collapse classification**: The registry defines "MCP tool collapse classification" but the chapter doesn't discuss how MCP tools are handled during context compaction.

## Verdict: REVISE
4 uncited brief files (2 prompt.ts, 2 UI.tsx files are minor; McpAuthTool.ts is a significant gap). 2-3 uncovered topics (McpAuthTool internals, ToolSearch matching details, MCP collapse classification).
