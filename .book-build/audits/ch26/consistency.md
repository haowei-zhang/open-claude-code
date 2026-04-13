# Consistency Audit: Chapter 26 — Memdir: Tiered Memory on Disk

## Verdict: PASS

### Term Conflicts

None. All registered terms used in this chapter are used with their canonical definitions:
- "memory" — used correctly as persistent note in memdir
- "memdir" — used correctly as the disk-based tiered memory subsystem
- "subagent" — used correctly (extraction agent is forked subagent)
- "frontmatter" — used correctly (YAML header on memory files)
- "feature gate" — used correctly (KAIROS, TEAMMEM gates)
- "compaction" — used correctly (session cap reset after compaction)

### Voice Drift

None detected. The chapter is written in present tense, descriptive, cite-heavy style consistent with house style.

### Proposed New Terms

1. **side query**: A secondary API call using a smaller model (typically Sonnet) to evaluate or select items without consuming the main model's context budget, used in memdir for relevance selection.
2. **entrypoint**: The MEMORY.md index file that is always loaded into context, serving as the compact catalog of available memory topic files, capped at 200 lines and 25KB.
3. **staleness warning**: A text injection appended to recalled memories older than one day via memoryFreshnessText(), reminding the model to verify claims before asserting them as fact.
