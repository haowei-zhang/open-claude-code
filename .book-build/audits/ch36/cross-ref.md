# Cross-Reference Audit: Chapter 36 - The Hook Schema and Lifecycle Events

## Required HER References

1. **§5 Pattern 12 Deterministic Lifecycle Hooks** - FOUND. Chapter explicitly references Pattern 12 in Overview and "Where cc diverges" sections. Cites shell commands at lifecycle points, PreToolUse/PostToolUse/SessionStart.

2. **§7 Configuration Surfaces** - FOUND. Chapter references "HER Section 7.5" explicitly, identifies hooks as one of six configuration surfaces. Discusses how hooks are a configuration surface.

## Divergence Section

The "Where cc diverges from the published pattern" section is substantive and covers:
1. Four persistable types + fifth programmatic type vs shell-only pattern
2. Prompt/agent/HTTP hook types as generalizations
3. cc not shipping default hooks (deliberate choice)
4. 23 lifecycle events beyond pattern's "25+ lifecycle points" (note: actual count issue)
5. Task coordination events beyond original pattern
6. `if` condition as pre-spawn filter (not in HER pattern)

Divergence section word count: ~380 words (well above 150 minimum)

## Issues

- Chapter claims "23 lifecycle events" but the actual source has 27 events. This is a factual inaccuracy in the HER engagement, not a missing ref.
- Chapter says events "go well beyond the '25+ lifecycle points' mentioned in HER Pattern 12" but then claims only 23, which is fewer than HER's 25+ claim. This is contradictory.
