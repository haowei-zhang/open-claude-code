# Diagrams Audit: Chapter 8 — Talking to Anthropic: Streaming

## Diagram Extraction

### Diagram 1: Streaming request lifecycle (line ~96)
```mermaid
sequenceDiagram
    participant QL as Query Loop
    participant Claude as claude.ts
    participant API as Anthropic API
    participant Retry as withRetry

    QL->>Claude: streamMessages(params)
    Claude->>Claude: Assemble request (model, betas, tools)
    Claude->>Retry: withRetry(streamFn)
    Retry->>API: HTTP POST /v1/messages
    API-->>Retry: SSE stream
    Retry-->>Claude: Parsed events
    Claude->>Claude: Accumulate usage
    Claude->>Claude: Detect cache break
    Claude-->>QL: Yield StreamEvents
    alt API error
        Retry->>Retry: Evaluate retry policy
        Retry->>API: Retry request
    end
```
- **Type**: sequenceDiagram — valid
- **Nodes/Actors**: 4 (QL, Claude, API, Retry) — ≥ 3, not trivial
- **Edge operators**: `->>`, `-->>` — valid for sequenceDiagram
- **Balanced brackets**: Yes
- **No Unicode arrows or smart quotes**: Yes
- **Verdict**: VALID, USEFUL

### Diagram 2: Request lifecycle with fallback (line ~155)
```mermaid
stateDiagram-v2
    [*] --> AssembleRequest: streamMessages called
    AssembleRequest --> SendRequest: Request ready
    SendRequest --> Streaming: 200 OK
    SendRequest --> RetryableError: 529/429/timeout
    SendRequest --> FatalError: 400/401/403
    RetryableError --> SendRequest: Backoff + retry
    RetryableError --> Fallback: Max retries exceeded
    Fallback --> AssembleRequest: Switch to fallback model
    Streaming --> AccumulateUsage: Stream complete
    AccumulateUsage --> CacheBreakCheck: Usage received
    CacheBreakCheck --> [*]: Yield to query loop
    FatalError --> [*]: Propagate error
```
- **Type**: stateDiagram-v2 — valid
- **Nodes/Actors**: 6 (AssembleRequest, SendRequest, Streaming, RetryableError, FatalError, AccumulateUsage, CacheBreakCheck, Fallback) — ≥ 3, not trivial
- **Edge operators**: `-->` — valid for stateDiagram-v2
- **Balanced brackets**: Yes
- **No Unicode arrows or smart quotes**: Yes
- **Verdict**: VALID, USEFUL

## Summary

- Total diagrams: 2
- Required minimum: 2 (brief requires 3: (a) sequenceDiagram of streaming request with retry, (b) stateDiagram-v2 of request lifecycle including fallback, (c) flowchart of usage accounting and cost hooks)
- Diagram (c) flowchart of usage accounting and cost hooks: **MISSING**
- Invalid diagrams: 0
- Trivial diagrams: 0
