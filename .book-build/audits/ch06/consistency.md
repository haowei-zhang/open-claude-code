# Consistency Audit: Chapter 6 — `main.tsx` and the Command Router

## Term Verification

### Registered Terms Found in Chapter
- **harness**: Used correctly (referring to the outer runtime layer)
- **feature gate**: Used correctly (referring to compile-time boundary via feature())
- **permission mode**: Used correctly (referring to named configurations)
- **hook**: Used correctly (referring to deterministic lifecycle event handlers)
- **fast-path routing**: Used correctly (referring to the pattern in cli.tsx)
- **deferred tool**: Not used in this chapter
- **tool**: Used correctly
- **session**: Used correctly

### Term Conflicts
- **"sub-agent"** (line 37) vs canonical "subagent" — Found one instance where the chapter uses "sub-agent" hyphenated. The canonical term in terminology.json is "subagent" (no hyphen).

### Voice Drift
- The chapter is written in present tense, descriptive, cite-heavy style consistent with the house style. No voice drift detected.

## Proposed New Terms

1. **pending state pattern** — A module-scope variable with a `feature()` gate that is set during argv parsing and consumed later in the action handler, enabling dead-code elimination of feature-gated code paths.
2. **eager settings loading** — The pattern of parsing CLI flags before the Commander program is constructed, necessary when flags affect initialization behavior.
3. **content-hash temp path** — A temporary file path derived from a content hash rather than a random UUID, used to preserve API prompt cache hits across process boundaries.
