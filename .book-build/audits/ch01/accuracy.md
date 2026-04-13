# Accuracy Audit: Chapter 01

## Citation Verification

### Inline Citations
1. `src/entrypoints/cli.tsx:L108-L162` — Text reference to bridge mode, daemon mode, etc. Lines 108-162 cover bridge mode; other fast-paths extend beyond this range. Claim is directionally correct.
2. `README.md:L46-L47` — Verified. Line 46 reads "Claude Code is not just a simple CLI. It's a massive **785KB `main.tsx`**..."
3. `src/entrypoints/cli.tsx:L286-L298` — Text reference to startup sequence. Off by one: the "No special flags detected" comment is at L287, not L286. Content is correct.

### Code Snippets
1. `src/entrypoints/cli.tsx:L33-L42` — The `if` statement is shown as a single line but the actual source uses multi-line formatting (`if (\n  args.length === 1 &&\n  (...) \n) {`). **DRIFT**: identifiers match but whitespace differs.
2. `src/entrypoints/cli.tsx:L16-L26` — Content matches actual source. The eslint-disable comment at L20 is omitted (acceptable trim). Array is truncated (acceptable). **VERBATIM** with trim.
3. `src/main.tsx:L9-L20` — **WRONG LINE NUMBERS**: The snippet includes the comment block at lines 1-8, but the citation says L9-L20. The actual L9 is `import { profileCheckpoint, profileReport }...`. Content shown is from L1-L16. Identifiers match; line numbers are wrong.
4. `src/entrypoints/cli.tsx:L288-L298` — Includes the comment from L287 (off by one). Code content matches exactly. **DRIFT**: minor line-number offset.

## Factual Claims
- "785KB main.tsx" — Consistent with README. Verified.
- "4,683-line main.tsx" — Cannot fully verify without reading entire file, but consistent with README description.
- "Chaofan Shou (@Fried_rice)" — Verified from README.
- "March 2026" for sourcemap leak — README says "March 31st, 2026". Verified.
- "24.9 percentage point spread" — Consistent with HER section 17 excerpt. Verified.
- "Mitchell Hashimoto... February 2026" — Consistent with HER section 1 excerpt. Verified.
- All quantitative metrics (TerminalBench, SWE-bench, METR, etc.) — Consistent with HER section 17 excerpt. Verified.

## Uncited Sources
None. All three source files (src/main.tsx, src/entrypoints/cli.tsx, README.md) are cited.
