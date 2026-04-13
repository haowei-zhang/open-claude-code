# Accuracy Audit Report — Chapter 5

## Summary

Verdict: **revise** (4 snippet drift issues, no hallucinated snippets, no bad citations)

## Citation Verification

All 25 citations reference files that exist and line numbers are approximately valid. No files are missing or wrongly named.

## Snippet Verification (5 total)

1. **src/bootstrap/state.ts:L45-L257** — Verbatim. The State type definition matches the source exactly (fields verified against lines 45-257).

2. **src/utils/settings/settings.ts:L538-L547** — Drift. The `settingsMergeCustomizer` function content matches but indentation differs (2-space in chapter vs 4-space in source). Line numbers are approximate.

3. **src/utils/settings/settings.ts:L91-L112** — Drift. The drop-in directory reading code matches in substance but formatting/indentation differs. Actual code starts at line 93, not 91.

4. **src/entrypoints/cli.tsx:L33-L43** — Drift. The `main()` function version check is reformatted (if condition on one line vs multi-line in source). Missing `biome-ignore` comment present in source.

5. **src/entrypoints/cli.tsx:L20-L26** — Drift. The ablation baseline block has different array formatting (3 lines in chapter vs 9 lines in source with per-element eslint comments). Should be L21, not L20.

## Uncited Sources

None — all 4 source files in the brief are cited.

## Factual Claims

All factual claims checked against source code are supported. The descriptions of TLS certificate ordering, memoized init, fail-open proxy, and settings cascade are accurate.
