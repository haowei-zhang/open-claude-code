# Diagrams Audit Report for Chapter 16

## Diagram 1: WebFetch flowchart (line 119)

```mermaid
flowchart TD
    A[Model requests WebFetch] --> B{Permission check}
    B -->|denied| C[Return permission error]
    B -->|allowed| D{isPreapprovedUrl}
    D -->|yes| E[Fetch without prompt]
    D -->|no| F[Fetch with prompt]
    E --> G[Convert to markdown]
    F --> G
    G --> H[Apply prompt to content]
    H --> I[Return result + metadata]
```

- **Type**: flowchart (valid)
- **Nodes**: 9 (A through I) - not trivial
- **Edge operators**: `-->` (valid for flowchart)
- **Brackets**: balanced
- **No Unicode arrows or smart quotes**
- **Verdict**: valid and useful

## Diagram 2: SSRF sequenceDiagram (line 193)

```mermaid
sequenceDiagram
    participant Hook as HTTP Hook
    participant SSRF as ssrfGuardedLookup
    participant DNS as dns.lookup
    participant Socket as TCP Socket

    Hook->>SSRF: lookup("169.254.169.254", ...)
    SSRF->>SSRF: isIP("169.254.169.254") = 4
    SSRF->>SSRF: isBlockedV4 -> true (link-local)
    SSRF-->>Hook: ERR_HTTP_HOOK_BLOCKED_ADDRESS

    Hook->>SSRF: lookup("docs.python.org", ...)
    SSRF->>DNS: dnsLookup("docs.python.org", {all: true})
    DNS-->>SSRF: [{address: "151.101.1.69", family: 4}]
    SSRF->>SSRF: isBlockedV4("151.101.1.69") -> false
    SSRF->>Socket: connect to 151.101.1.69
```

- **Type**: sequenceDiagram (valid)
- **Participants**: 4 (Hook, SSRF, DNS, Socket) - not trivial
- **Edge operators**: `->>`, `-->>` (valid for sequenceDiagram)
- **Brackets**: balanced
- **No Unicode arrows or smart quotes**
- **Verdict**: valid and useful

## Summary

- 2 diagrams total
- Required minimum: 2 (meets)
- Brief requires: 2 ((a) flowchart of WebFetch with SSRF guard, (b) sequenceDiagram of web-content sanitization)
- Both required diagram types present
- All diagrams syntactically valid and non-trivial
