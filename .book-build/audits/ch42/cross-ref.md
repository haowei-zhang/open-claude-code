# Cross-Reference Audit: Chapter 42 — The Ink Renderer and Terminal Engine

## Required HER References

The chapter brief requires:
- §21 reference architecture UX layer

## Verification

The chapter cites HER Section 21 in the "Selection scroll-following: capture and shift" subsection, near the end of the "Where cc diverges from the published pattern" section:

> "HER Section 21's reference architecture for Layer 7 (the UX layer) emphasizes that 'the harness is the outer runtime layer that orchestrates the model's interactions with tools, permissions, hooks, memory, and the user.' The Ink renderer is the primary orchestration surface between the model and the user, and its performance characteristics directly affect the perceived responsiveness of the entire system."

This reference is:
1. **Present**: The chapter does cite §21.
2. **Real section**: Section 21 exists in the HER report and contains the Layer 7 UX reference architecture.
3. **Accurate**: The quoted characterization of Layer 7 is consistent with the HER excerpt.

However, the citation is a single sentence embedded in the middle of a subsection about selection scroll-following, which is not the most natural placement for the primary HER engagement. The chapter should more substantively engage with the §21 reference architecture across the broader chapter, not just in a single aside.

## Divergence Section

The "Where cc diverges from the published pattern" section is present and substantive. It covers:
1. Style transition caching and the yellow-fg-via-inverse trick
2. In-process server cleanup (off-topic)
3. Custom reconciler vs. Ink's built-in
4. Screen differencing vs. full repaint
5. Interned pools vs. string-per-cell
6. Packed cell layout: two Int32s per cell
7. Damage tracking for limited diff
8. Clip regions and overflow handling
9. Shift operations for scroll
10. NoSelect: exclusion regions for gutters
11. Selection background: solid color vs. inverse
12. Yoga layout integration and onComputeLayout
13. Render scheduling: microtask deferral and throttling
14. Terminal handoff: enterAlternateScreen and exitAlternateScreen
15. Selection scroll-following: capture and shift
16. log-update: the terminal write provider

The divergence section is extensive and substantive (well over 3000 words), covering multiple concrete divergences from the standard approach. This easily exceeds the 150-word minimum.

## Issues

1. The HER reference to §21 is a single sentence and feels bolted on rather than integrated. The chapter would benefit from engaging with §21's UX layer framework more substantively — for example, discussing how each of the Ink renderer's subsystems maps to the "observation" or "interaction" dimensions of the UX layer.

2. The "In-process server cleanup" subsection is off-topic for this chapter (it discusses MCP server cleanup, not the Ink renderer).

## Summary

- Required refs found: 1/1
- Divergence section length: ~3500 words (well above 150-word minimum)
- Issues: 1 (weak HER engagement despite presence of required ref)
