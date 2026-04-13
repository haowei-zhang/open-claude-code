# Consistency Audit: Chapter 24 — Dream Tasks: Background Consolidation

## Term Consistency Check

| Term in Registry | Usage in Chapter | Consistent? |
|------------------|-----------------|-------------|
| tool | Used correctly ("tool_use blocks", "Edit and Write tool calls") | YES |
| subagent | Used correctly ("forked subagent", "background forked subagent") | YES |
| fork | Used correctly ("forked subagent", "forked agent", "forked dream agent") | YES |
| task | Used correctly ("DreamTask", "background task", "task state") | YES |
| memory | Used correctly ("memory files", "memory directory", "MEMORY.md index") | YES |
| memdir | Referenced correctly ("memdir directory") | YES |
| session | Used correctly ("session directory", "session scan", "current session") | YES |
| compaction | Not used in this chapter (appropriate — chapter is about consolidation, not compaction) | N/A |
| hook | Not directly used | N/A |
| permission mode | Not used | N/A |
| feature gate | Used correctly ("feature-gated") | YES |
| GrowthBook | Used correctly ("GrowthBook feature flag", "GrowthBook cache") | YES |
| observation masking | Not used | N/A |

## Alternate Name Check

- "auto-dream" (prose) vs "autoDream" (code) — acceptable convention (hyphenated in prose, camelCase in code)
- "dream agent" used consistently throughout
- "consolidation lock" used consistently throughout
- No instances of "sub-agent" vs "subagent" conflict
- No instances of "tool call" vs "tool use" conflict

## Voice Drift

The chapter is written in present tense, descriptive, cite-heavy style — consistent with the house style. No voice drift detected.

## Proposed New Terms

1. **dream consolidation**: A background process that reviews, deduplicates, and prunes memory files during idle time, implemented via a three-gate system (time, sessions, lock) and a forked subagent.

2. **consolidation lock**: A filesystem-based lock file whose mtime serves as the lastConsolidatedAt timestamp and whose body contains the holder's PID, preventing concurrent consolidation across processes.

3. **three-gate cascade**: A pattern for background task scheduling where progressively more expensive checks (time, workload, lock) are evaluated in order, minimizing wasted computation when upstream gates would have rejected the task.

## Summary

- Term conflicts: 0
- Voice drift: none
- Proposed new terms: 3
