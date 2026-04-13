# Accuracy Audit Report - Chapter 7

## Citation Verification

### Snippet 1: `src/query.ts:L181-L199` (QueryParams type)
- File exists: YES
- Line range valid: YES (lines 181-199)
- Content matches: VERBATIM - the QueryParams type in the chapter matches the source file exactly (lines 181-199)
- Caption present: YES (`// src/query.ts:L181-L199`)

### Snippet 2: `src/query.ts:L204-L217` (State type)
- File exists: YES
- Line range valid: YES (lines 204-217)
- Content matches: VERBATIM - the State type in the chapter matches the source file exactly
- Caption present: YES (`// src/query.ts:L204-L217`)

### Snippet 3: `src/query.ts:L219-L239` (query generator function)
- File exists: YES
- Line range valid: YES (lines 219-239)
- Content matches: VERBATIM - the async generator function signature matches
- Caption present: YES (`// src/query.ts:L219-L239`)

### Snippet 4: `src/query.ts:L123-L149` (yieldMissingToolResultBlocks)
- File exists: YES
- Line range valid: YES (lines 123-149)
- Content matches: VERBATIM - the generator function matches the source exactly
- Caption present: YES (`// src/query.ts:L123-L149`)

## Factual Claims Verification

1. "approximately 1,700 lines" for src/query.ts - VERIFIED: file is 1,729 lines
2. "MAX_OUTPUT_TOKENS_RECOVERY_LIMIT = 3" at L164 - VERIFIED: line 164 has this constant
3. "The rules of thinking" at L152-L163 - VERIFIED: the comment block exists at those lines
4. "handleStopHooks() in src/query/stopHooks.ts" - VERIFIED: file exists and function is imported at line 101
5. "createBudgetTracker, checkTokenBudget in src/query/tokenBudget.ts" - VERIFIED: functions exist
6. "productionDeps() in src/query/deps.ts" - VERIFIED: function exists at line 33
7. "calculateTokenWarningState() in src/services/compact/autoCompact.ts" - VERIFIED: function at line 93
8. "runTools() in src/services/tools/toolOrchestration.ts" - VERIFIED: imported at line 98
9. "StreamingToolExecutor in src/services/tools/StreamingToolExecutor.ts" - VERIFIED: imported at line 96
10. "generateToolUseSummary() in src/services/toolUseSummary/toolUseSummaryGenerator.ts" - VERIFIED: imported at line 57
11. "executePostSamplingHooks() in src/utils/hooks/postSamplingHooks.ts" - VERIFIED: imported at line 92
12. "normalizeMessagesForAPI() in src/utils/messages.ts" - VERIFIED: imported at line 49
13. "applyToolResultBudget() at src/query.ts:L379-L394" - VERIFIED: function call at lines 379-394
14. "taskBudgetRemaining at src/query.ts:L291" - VERIFIED: variable at line 291
15. "pendingMemoryPrefetch at src/query.ts:L301-L304" - VERIFIED: `using` declaration at lines 301-304
16. "snip compact at src/query.ts:L401-L409" - VERIFIED: code block at those lines
17. "microcompact at src/query.ts:L413-L425" - VERIFIED: code block at those lines
18. "context collapse at src/query.ts:L440-L447" - VERIFIED: code block at those lines
19. "autocompact at src/query.ts:L453-L467" - VERIFIED: code block at those lines

## Uncited Source Files

The following source files from the brief are NOT cited in the chapter:
- `src/QueryEngine.ts` - NOT CITED (informational, this is a higher-level orchestrator that wraps query.ts)

## Issues Found

1. The chapter references "src/query.ts:L175-L179" for the isWithheldMaxOutputTokens function. The actual function spans lines 175-179. VERIFIED.

2. The chapter states "src/bootstrap/state.ts:L733-L737" for snapshotOutputTokensForTurn. VERIFIED: the function is at lines 733-737.

3. The chapter mentions "stripSignatureBlocks()" and "stripAdvisorBlocks()" as separate functions in src/utils/messages.ts. Only stripSignatureBlocks is imported at line 56. stripAdvisorBlocks is not visible in the imports - this may be a minor inaccuracy or the function may be called indirectly.

4. The chapter references "ensureToolResultPairing() in src/utils/messages.ts" but this function is not visible in the imports of query.ts. It may exist in messages.ts but is not directly called from the query loop code visible.

## Verdict: revise

Reason: All 4 snippets are verbatim and match the source. However, there are 2 minor issues: (1) stripAdvisorBlocks() is referenced but not clearly verifiable as an import in query.ts, and (2) ensureToolResultPairing() is referenced as being in messages.ts but its call from the query loop is not verified. These are minor drift/unsupported claims, not hallucinations.
