# Accuracy Audit Report - Chapter 56

## Summary

Chapter 56 is a meta-chapter (no source_files in brief). It proposes new contracts and a roadmap, citing existing cc source files as evidence for gaps.

## Citation Verification

### Snippet 1: `src/cost-tracker.ts:L278-L284`
- File exists: YES
- Line range valid: YES (lines 278-284 match `addToTotalSessionCost`)
- Content matches: VERBATIM - the function signature and body match exactly
- Surrounding claim ("purely additive, no threshold check, no budget gate") is SUPPORTED

### Snippet 2: `src/tools/AskUserQuestionTool/AskUserQuestionTool.tsx:L209-L222`
- File exists: YES
- Line range valid: YES (lines 209-222 match the `call()` method)
- Content matches: VERBATIM - the async call method with questions/answers/annotations matches
- Surrounding claim ("no confidence data, no metadata about agent's decision context") is SUPPORTED

### Snippet 3: `src/services/analytics/index.ts:L72-L78`
- File exists: YES
- Line range valid: YES (lines 72-78 match `AnalyticsSink` type)
- Content matches: VERBATIM - the type definition with logEvent/logEventAsync matches
- Surrounding claim ("no trace context, no span linkage") is SUPPORTED

### Snippet 4: `src/services/compact/autoCompact.ts:L258-L265`
- File exists: YES
- Line range valid: YES (lines 258-265 match the circuit breaker)
- Content matches: VERBATIM - the consecutive failures check matches
- Surrounding claim ("cost-avoidance circuit breaker, not quality circuit breaker") is SUPPORTED

### Snippet 5: `src/hooks/useCanUseTool.tsx:L126-L131`
- File exists: YES
- Line range valid: YES (line 126 contains the speculative classifier race logic)
- Content matches: DRIFT - the actual code at line 126 is a single-line conditional that spans multiple conditions, but the logic (feature check, pendingClassifierCheck, BASH_TOOL_NAME, awaitAutomatedChecksBeforeDialog) is accurate. The indentation/whitespace differs from what's shown in the chapter vs the actual minified/bundled code.
- Surrounding claim ("speculative classifier races dialog") is SUPPORTED

### Snippet 6: `src/tools/AskUserQuestionTool/AskUserQuestionTool.tsx:L182-L188`
- File exists: YES
- Line range valid: YES (lines 182-188 match `checkPermissions`)
- Content matches: VERBATIM - the behavior: 'ask' return matches
- Surrounding claim ("all questions are Tier 3") is SUPPORTED

### Snippet 7: `src/cost-tracker.ts:L71-L80`
- File exists: YES
- Line range valid: YES (lines 71-80 match `StoredCostState` type)
- Content matches: VERBATIM - the type definition matches exactly
- Surrounding claim ("tracks per-model usage but no per-task caps") is SUPPORTED

### Snippet 8: `src/cost-tracker.ts:L130-L137`
- Referenced in text (not a fenced snippet) - "restoreCostStateForSession in src/cost-tracker.ts:L130-L137"
- File exists: YES
- Line range valid: YES (lines 130-137 match `restoreCostStateForSession`)
- Claim SUPPORTED

### Snippet 9: `src/cost-tracker.ts:L143-L175`
- Referenced in text - "saveCurrentSessionCosts function at src/cost-tracker.ts:L143-L175"
- File exists: YES
- Line range valid: YES (lines 143-175 match `saveCurrentSessionCosts`)
- Claim SUPPORTED

### Snippet 10: `src/services/analytics/index.ts:L95-L123`
- Referenced in text - "attachAnalyticsSink function in src/services/analytics/index.ts:L95-L123"
- File exists: YES
- Line range valid: YES (lines 95-123 match `attachAnalyticsSink`)
- Claim SUPPORTED

### Snippet 11: `src/services/analytics/index.ts:L133-L144`
- Referenced in text - "logEvent function at src/services/analytics/index.ts:L133-L144"
- File exists: YES
- Line range valid: YES (lines 133-144 match `logEvent`)
- Claim SUPPORTED

### Snippet 12: `src/cost-tracker.ts:L228-L244`
- Referenced in text - "formatTotalCost function in src/cost-tracker.ts:L228-L244"
- File exists: YES
- Line range valid: YES (lines 228-244 match `formatTotalCost`)
- Claim SUPPORTED

### Uncited References
- `src/services/compact/autoCompact.ts:L70` - referenced in text, valid (MAX_CONSECUTIVE_AUTOCOMPACT_FAILURES)
- `src/bootstrap/state.ts` - referenced in text (markPostCompaction), valid
- `src/utils/hooks/ssrfGuard.ts` - referenced in text, valid
- `src/utils/tasks.ts` - referenced in text, valid

## Issues Found

1. Snippet 5 (useCanUseTool.tsx:L126-L131): DRIFT - the actual code formatting differs. The logic is correct but the exact whitespace/formatting doesn't match character-by-character. The actual code is more condensed on line 126 than shown in the chapter.

No hallucinated snippets found. All line references point to real code that supports the surrounding claims.
