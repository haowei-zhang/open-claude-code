# Consistency Audit: Chapter 40 - MCP: Clients, Transports, and Lifecycle

## Terminology Conflicts

No term conflicts found. All registered terms used correctly:
- "tool" - used consistently with registry definition
- "subagent" - not used in this chapter (appropriate)
- "skill" - used correctly in context of MCP skill shell execution
- "hook" - used correctly in context of MCP hooks
- "deferred tool" - used correctly for MCP tool lazy loading
- "ToolSearch" - used correctly for progressive tool expansion
- "observation masking" - used correctly for MCP output truncation
- "progressive tool expansion" - used correctly for deferred MCP tool loading
- "SSRF guard" - used correctly for MCP server network protection

## Voice Drift

No voice drift detected. The chapter maintains:
- Present tense throughout
- Descriptive, cite-heavy register
- Consistent with house style

## Proposed New Terms

1. **MCP server connection** - A discriminated union tracking the lifecycle state of an MCP server connection through five states: pending, connected, failed, needs-auth, and disabled.
2. **MCP transport** - The communication channel between cc and an MCP server, implemented as stdio (child process), SSE, HTTP, WebSocket, or SDK (in-process).
3. **Cross-App Access** - An enterprise authentication pattern where MCP servers authenticate through a corporate Identity Provider (IdP) rather than their own OAuth flow, enabling SSO across all MCP servers.
4. **MCP tool collapse classification** - The heuristic categorization of MCP tools for compaction, determining which tool results can be safely summarized during context compaction.
5. **server signature deduplication** - The signature-based approach to preventing duplicate MCP server configurations using command arrays or unwrapped URLs as keys.

## Verdict: PASS
