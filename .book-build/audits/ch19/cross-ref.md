# Cross-Reference Audit: Chapter 19 — Sync vs Fork vs Remote: The Execution Modes

## Required HER References

1. **§9.1 Three Tiers of Multi-Agent Orchestration** — FOUND. The Overview section explicitly states "These three tiers map directly to HER Section 9.1's three tiers of multi-agent orchestration: in-process subagents, local orchestrators, and cloud async." The Control Flow section examines each tier. The Divergence section references HER Section 9.1.

2. **§6.5 Context Anxiety** — FOUND. The Edge cases section has a dedicated bullet point on "Fork child context rot" that cites "HER Section 6.5 (Context Anxiety) notes that models prematurely wrap up near perceived context limits." The Divergence section also references HER Section 6.5 in a substantive paragraph.

## Divergence Section

The "Where cc diverges from the published pattern" section is present and substantive. It contains four distinct divergence points:
1. Tier fluidity (sync agents can be auto-backgrounded)
2. Fork mode pushing full context despite Context Anxiety guidance
3. Bubble permission model not described in HER
4. Lack of formal join in fork-join parallelism

Word count of divergence section: approximately 260 words. This exceeds the 150-word minimum.

## Issues

None. Both required HER references are present and substantively engaged. The divergence section is substantive.

## Summary

- Required refs found: 2/2
- Divergence section present: YES
- Divergence section word count: ~260
