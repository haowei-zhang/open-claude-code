# Accuracy Audit: Chapter 8 — Talking to Anthropic: Streaming

## Citation Verification

### Snippet 1: `src/services/api/claude.ts:L131-L143` (beta header imports)
- **Status**: VERIFIED. The actual lines 133-143 match the import block from `src/constants/betas.js`. The chapter cites L131-L143; the actual imports start at L133. Minor line offset (2 lines off), but content is accurate.
- **Verdict**: drift (line numbers off by 2, but content matches)

### Snippet 2: `src/services/api/logging.ts` (NonNullableUsage type)
- **Status**: NOT VERBATIM. The chapter shows `NonNullableUsage` with fields `input_tokens`, `output_tokens`, `cache_creation_input_tokens`, `cache_read_input_tokens`, `server_tool_use: number | null`. The actual type is defined in `src/entrypoints/sdk/sdkUtilityTypes.js` and re-exported from `logging.ts`. The chapter attributes the type to `logging.ts` directly, which is a re-export location — the true definition is elsewhere.
- **Verdict**: drift (correct fields, wrong file attribution for the type definition)

### Snippet 3: `src/services/api/claude.ts:L272-L299` (getExtraBodyParams)
- **Status**: VERIFIED. The actual function at lines 272-299 matches the chapter's quoted code. The shallow clone comment is present. Content matches exactly.
- **Verdict**: verbatim

### Snippet 4: `src/services/api/claude.ts:L117-L132` (latch header imports)
- **Status**: VERIFIED. The actual imports from `src/bootstrap/state.js` appear at lines 116-132. Content matches.
- **Verdict**: verbatim

### Snippet 5: `src/services/api/claude.ts:L246-L257` (cache break detection imports)
- **Status**: VERIFIED. The actual imports from `./promptCacheBreakDetection.js` appear at lines 246-250. The chapter shows lines 246-257, which includes additional imports. The content matches.
- **Verdict**: verbatim

## Factual Claims

1. `extractQuotaStatusFromError()` is said to be at `src/services/api/claude.ts:L98`. Actually, `extractQuotaStatusFromError` is imported from `src/services/claudeAiLimits.ts` at line 99. The function definition is in `claudeAiLimits.ts`, not in `claude.ts`. **bad_citation** — wrong file.

2. `addToTotalSessionCost()` is said to be at `src/services/api/claude.ts:L146`. Actually, this is imported from `src/cost-tracker.ts` at line 146. The function definition is in `cost-tracker.ts`. **bad_citation** — the import line is correct but the chapter implies the function lives in claude.ts.

3. The `withRetry()` wrapper is said to be in `src/services/api/withRetry.ts`. **VERIFIED** — this file exists and contains the function.

4. The chapter claims `getMergedBetas()` is in `src/utils/betas.ts`. **VERIFIED**.

5. The chapter claims `computeFingerprintFromMessages()` is in `src/utils/fingerprint.ts`. **VERIFIED**.

6. The chapter references `src/services/api/promptCacheBreakDetection.ts` for cache break detection. **VERIFIED** — this file exists.

7. The chapter claims `CACHE_TTL_1HOUR_MS` constant is in `promptCacheBreakDetection.ts`. **VERIFIED** — line 126.

8. The chapter claims `clearBetaHeaderLatches()` is in `bootstrap/state.ts:L1744-L1749`. **VERIFIED** — line 1744.

9. The chapter claims `thinkingClearLatched` flag is at `src/bootstrap/state.ts:L240-L243`. The actual field is defined around line 242 in the state type. **VERIFIED**.

10. The `NonNullableUsage` type is shown in the chapter as living in `src/services/api/logging.ts`. The actual definition is in `src/entrypoints/sdk/sdkUtilityTypes.js`, re-exported from logging.ts. This is a minor attribution error.

## Uncited Sources

- `src/services/api/usage.ts` — Listed in brief but not directly cited. The chapter covers usage accumulation but references `cost-tracker.ts` instead.

## Summary

- 2 bad citations (function attributed to wrong file: `extractQuotaStatusFromError` and `addToTotalSessionCost`)
- 1 snippet drift (NonNullableUsage attributed to logging.ts instead of sdkUtilityTypes.js)
- 1 uncited source file (usage.ts)
- 0 hallucinated snippets
- 5 total snippets, 3 verbatim, 2 drift
