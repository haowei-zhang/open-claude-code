# Diagrams Audit: Chapter 37

## Extracted Mermaid Diagrams

### Diagram 1: Prompt hook sequence diagram (line 161)
```mermaid
sequenceDiagram
    participant QL as Query Loop
    participant EPH as execPromptHook
    participant Model as Haiku Model
    QL->>EPH: invoke(hook, jsonInput, signal)
    EPH->>EPH: addArgumentsToPrompt(hook.prompt, jsonInput)
    EPH->>EPH: createCombinedAbortSignal(parent, timeout)
    EPH->>Model: queryModelWithoutStreaming(systemPrompt, messages)
    Model-->>EPH: text response
    EPH->>EPH: safeParseJSON + hookResponseSchema().safeParse
    alt ok: true
        EPH-->>QL: {outcome: "success"}
    else ok: false
        EPH-->>QL: {outcome: "blocking", preventContinuation: true}
    else parse error
        EPH-->>QL: {outcome: "non_blocking_error"}
    end
```
- Type: sequenceDiagram (valid)
- 3 participants (valid, not trivial)
- Balanced brackets (valid)
- Valid edge operators: ->>, -->> (valid for sequenceDiagram)
- No Unicode arrows or smart quotes (valid)
- Verdict: valid, useful

### Diagram 2: HTTP hook sequence diagram with SSRF (line 235)
```mermaid
sequenceDiagram
    participant QL as Query Loop
    participant EHH as execHttpHook
    participant Policy as getHttpHookPolicy
    participant DNS as ssrfGuardedLookup
    participant Remote as Remote Server
    QL->>EHH: invoke(hook, jsonInput, signal)
    EHH->>Policy: getHttpHookPolicy()
    Policy-->>EHH: {allowedUrls, allowedEnvVars}
    EHH->>EHH: urlMatchesPattern check
    EHH->>EHH: interpolateEnvVars(headers, allowedEnvVars)
    EHH->>EHH: sanitizeHeaderValue(interpolated)
    EHH->>DNS: ssrfGuardedLookup(hostname)
    DNS->>DNS: dns.lookup then isBlockedAddress?
    alt blocked
        DNS-->>EHH: ERR_HTTP_HOOK_BLOCKED_ADDRESS
        EHH-->>QL: {ok: false, error: "SSRF blocked"}
    else allowed
        DNS-->>EHH: resolved address
        EHH->>Remote: POST jsonInput (via axios)
        Remote-->>EHH: {status, body}
        EHH-->>QL: {ok, statusCode, body}
    end
```
- Type: sequenceDiagram (valid)
- 5 participants (valid, not trivial)
- Balanced brackets (valid)
- Valid edge operators: ->>, -->> (valid for sequenceDiagram)
- No Unicode arrows or smart quotes (valid)
- Verdict: valid, useful

## Required Diagrams from Brief
- (a) sequenceDiagram of a prompt hook — present as Diagram 1
- (b) sequenceDiagram of an HTTP hook with SSRF check — present as Diagram 2

Both required diagrams are present and valid.

## Verdict: pass
