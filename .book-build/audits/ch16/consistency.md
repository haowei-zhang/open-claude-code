# Consistency Audit Report for Chapter 16

## Terminology Conflicts

Checked all terms in terminology.json against chapter usage:

- **tool**: Used correctly throughout (as a "typed, schema-validated operation exposed to the model via the tool dispatch pipeline").
- **deferred tool**: Chapter mentions `shouldDefer: true` and describes it as "not included in the initial prompt and must be discovered via ToolSearchTool" - matches canonical definition.
- **ToolSearch**: Chapter references ToolSearchTool correctly.
- **dispatch pipeline**: Not used; chapter focuses on individual tool behavior, not the dispatch pipeline. No conflict.
- **permission mode**: Not referenced directly; chapter discusses permission rules but not modes. No conflict.
- **hook**: Chapter references "HTTP hooks" in context of the SSRF guard. Matches the definition (lifecycle event handler).
- **harness**: Not used in this chapter. No conflict.

### Conflict 1: "sub-agent" vs "subagent"
No occurrence. No conflict.

### Conflict 2: "tool call" vs "tool use"
- Chapter line 263: "Hallucinated Tool Calls" - this is a direct quote of the HER section title (6.12), so it is acceptable as a proper noun/reference. However, the chapter also uses "tool call" generically in a few places (e.g., "fabricates tool parameters, calls wrong APIs"). The terminology registry does not have an entry for "tool call" vs "tool use", so this is not a conflict.

### Conflict 3: "WebFetch" vs "WebFetchTool"
- Chapter uses both "WebFetchTool" (the implementation class name) and "WebFetch" (the tool name). This is consistent with the codebase where `WEB_FETCH_TOOL_NAME = 'WebFetch'` and the class is `WebFetchTool`. No conflict.

### Conflict 4: "SSRF guard" terminology
- The chapter refers to the module as "the SSRF guard" throughout. The terminology registry does not have an entry for SSRF guard. This is a new term introduced by the chapter.

## Voice Drift

The chapter is written in present tense, descriptive, cite-heavy style consistent with the house style. No voice drift detected.

## Proposed New Terms

1. **SSRF guard** - The dns.lookup-compatible function in src/utils/hooks/ssrfGuard.ts that blocks connections to private, link-local, and CGNAT address ranges, preventing HTTP hooks from reaching cloud metadata endpoints.
2. **preapproved URL** - A URL whose hostname is on the PREAPPROVED_HOSTS allowlist, allowing WebFetchTool to skip explicit per-request permission checks for known-safe documentation sites.
3. **domain-based permission rule** - A permission rule that matches web tool access at the hostname level using the `domain:hostname` pattern, enabling fine-grained access control without requiring full URL specification.
4. **IPv4-mapped IPv6 bypass** - An SSRF attack vector where a private IPv4 address is encoded as an IPv6 address using the ::ffff:X.X.X.X format, bypassing IPv4-only address checks.
5. **DNS-rebinding protection** - The pattern of validating DNS-resolved IP addresses and passing the validated address directly to the TCP socket, closing the window where an attacker could serve different IPs on successive DNS resolutions.
