# Consistency Audit for Chapter 41

## Summary
Checking terminology consistency against the terminology registry for chapter 41.

## Term Conflicts

1. **"deferred tool" vs usage**: The chapter uses "deferred tool" and "deferred loading" which aligns with the registered term "deferred tool" (a tool not included in the initial prompt but loaded on demand via ToolSearch). No conflict.

2. **"ToolSearch" vs "ToolSearchTool"**: The chapter uses both "ToolSearchTool" (the actual tool name) and "ToolSearch" (as shorthand). The registry defines "ToolSearch" as the system name. The dual usage is acceptable since "ToolSearchTool" refers to the tool implementation and "ToolSearch" to the system. No conflict.

3. **"passthrough permissions"**: The chapter uses this term repeatedly. Not registered in terminology.json. This is a new term introduced by this chapter.

4. **"progressive tool expansion"**: Used correctly per the registry definition. No conflict.

5. **"observation masking"**: The chapter uses this term in the context of binary blob persistence (line 409). The registry defines "observation masking" as "The technique of redacting or summarizing prior tool outputs to reduce token cost while preserving decision-relevant information." The chapter's usage is consistent with this definition. No conflict.

6. **"session retry" / "McpSessionExpiredError"**: Technical terms not in the registry, used consistently.

7. **"prompt assembly"**: Not used in this chapter. No conflict.

8. **"MCP server connection"**: The registry defines this as "A discriminated union tracking the lifecycle state of an MCP server connection through five states: pending, connected, failed, needs-auth, and disabled." The chapter mentions 'needs-auth' state. Consistent usage. No conflict.

9. **"MCP transport"**: Used consistently with registry definition. No conflict.

## Voice Drift
No voice drift detected. The chapter is written in present tense, descriptive, cite-heavy style consistent with the house style.

## Proposed New Terms

1. **"passthrough permission"**: A permission result type (behavior: 'passthrough') where the tool invocation requires user approval unless a rule explicitly allows it, used for MCP tools whose code is not controlled by the harness.

2. **"session-retry loop"**: A retry pattern in MCP tool calls that automatically retries once on McpSessionExpiredError after clearing the connection cache and obtaining a fresh client.

3. **"batched connection processing"**: The startup pattern of connecting to MCP servers in parallel groups partitioned by transport type (local vs remote) with different concurrency limits.

## Verdict: PASS
Zero term conflicts. No voice drift. 3 proposed new terms.
