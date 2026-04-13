# Cross-Reference Audit: Chapter 6 — `main.tsx` and the Command Router

## Required HER References

| Ref | Found | Location |
|-----|-------|----------|
| §7 Configuration Surfaces (entry flags as a surface) | YES | Overview paragraph, "Where cc diverges" section |

## Additional HER References Found

- §7.1 (CLAUDE.md / ETH Zurich finding) — cited in divergence section
- §7.5 (hooks as lifecycle events) — cited in divergence section
- §12 (security/trust) — cited in interactive mode setup and prefetchSystemContextIfSafe discussion
- §12.4 (supply chain attacks) — cited in plugin/skill initialization section

## Divergence Section

The "Where cc diverges from the published pattern" section is present and substantive at approximately 220 words. It covers:
1. The `--setting-sources` flag as a cc-specific extension
2. The migration system not described in HER
3. ETH Zurich finding on context files vs. cc's deferred prefetch approach
4. Hooks implementation through `processSessionStartHooks()` and `processSetupHooks()`

## Issues

None. All required HER refs are present, divergence section is substantive, and additional HER cross-references enrich the chapter.
