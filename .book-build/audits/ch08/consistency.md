# Consistency Audit: Chapter 8 — Talking to Anthropic: Streaming

## Term Verification

### Registered terms used in this chapter:
1. **tool** — used consistently with canonical definition (typed, schema-validated operation)
2. **compaction** — referenced correctly in context of cache busting
3. **observation masking** — used correctly, referencing HER section 13.3
4. **checkpoint-restore** — not used in this chapter
5. **permission mode** — not used in this chapter
6. **harness** — used correctly
7. **back-pressure** — used correctly (SSE back-pressure handling)
8. **feature gate** — referenced correctly in context of beta header gating
9. **deferred tool** — referenced correctly in context of ToolSearch
10. **ToolSearch** — referenced correctly

### Term conflicts:
1. **"sub-agent" vs "subagent"**: The chapter uses "sub-agents" on line 265 in the advisor section context. The canonical term is "subagent" (no hyphen). **CONFLICT**: should be "subagent".

### Voice drift:
- The chapter is written in present tense, descriptive style with heavy citation. Consistent with house style. No voice drift detected.

### Proposed new terms:
1. **beta header latching** — The pattern where beta headers stay on once activated to prevent cache busting. Distinct from general caching concepts.
2. **cache break** — The event where the API's prompt cache is invalidated, requiring full re-processing of input tokens. Specific to streaming API behavior.
3. **full jitter** — A retry backoff strategy where the delay is uniformly distributed between 0 and the current backoff interval, preventing thundering herd effects.
