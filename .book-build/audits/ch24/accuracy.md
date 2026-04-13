# Accuracy Audit: Chapter 24 — Dream Tasks: Background Consolidation

## Citation Verification

All major line-number citations in the chapter were checked against the actual source files:

### Verified Correct
- `src/services/autoDream/autoDream.ts:L58-L61` (AutoDreamConfig) — correct
- `src/services/autoDream/autoDream.ts:L63-L66` (DEFAULTS) — correct
- `src/services/autoDream/autoDream.ts:L73-L93` (getConfig) — correct
- `src/services/autoDream/autoDream.ts:L95-L100` (isGateOpen) — correct
- `src/services/autoDream/autoDream.ts:L125-L190` (three-gate cascade) — approximately correct
- `src/services/autoDream/autoDream.ts:L216-L221` (Bash restriction) — correct
- `src/services/autoDream/autoDream.ts:L238-L248` (inline completion) — correct
- `src/services/autoDream/autoDream.ts:L268-L270` (rollback on fork failure) — correct
- `src/services/autoDream/autoDream.ts:L281-L313` (progress watcher) — correct
- `src/services/autoDream/autoDream.ts:L319-L324` (executeAutoDream) — correct
- `src/services/autoDream/consolidationLock.ts:L16-L17` (LOCK_FILE) — correct
- `src/services/autoDream/consolidationLock.ts:L19` (HOLDER_STALE_MS) — correct
- `src/services/autoDream/consolidationLock.ts:L29-L36` (readLastConsolidatedAt) — correct
- `src/services/autoDream/consolidationLock.ts:L46-L84` (tryAcquireConsolidationLock) — correct
- `src/services/autoDream/consolidationLock.ts:L91-L108` (rollbackConsolidationLock) — correct
- `src/services/autoDream/consolidationLock.ts:L118-L124` (listSessionsTouchedSince) — correct
- `src/services/autoDream/consolidationLock.ts:L71` (mkdir) — correct
- `src/services/autoDream/consolidationLock.ts:L130-L140` (recordConsolidation) — correct
- `src/tasks/DreamTask/DreamTask.ts:L12` (MAX_TURNS) — correct
- `src/tasks/DreamTask/DreamTask.ts:L23` (DreamPhase) — correct
- `src/tasks/DreamTask/DreamTask.ts:L25-L41` (DreamTaskState) — correct
- `src/tasks/DreamTask/DreamTask.ts:L76-L104` (addDreamTurn) — correct
- `src/tasks/DreamTask/DreamTask.ts:L132-L157` (kill) — correct
- `src/services/autoDream/consolidationPrompt.ts:L14-L65` (buildConsolidationPrompt) — correct

### Issues Found
1. **Wrong line number**: `src/tasks/DreamTask/DreamTask.ts:L16-L19` (DreamTurn) — the `export type DreamTurn` declaration starts at L15, not L16. L16-L19 captures only `text`, `toolUseCount`, and `}`, missing the `export type` header.
2. **Wrong line number**: `src/tasks/DreamTask/DreamTask.ts:L44-L74` (registerDreamTask) — registerDreamTask starts at L52, not L44. L44 is in the middle of the isDreamTask function.
3. **Minor offset**: `src/services/autoDream/autoDream.ts:L164-L165` (exclude current session) — actual code at L163-L165 (off by one).

## Snippet Verification

| # | Location | Verdict | Notes |
|---|----------|---------|-------|
| 1 | autoDream.ts:L58-L61 (AutoDreamConfig) | verbatim | Exact match |
| 2 | autoDream.ts:L63-L66 (DEFAULTS) | verbatim | Exact match |
| 3 | DreamTask.ts:L25-L41 (DreamTaskState) | drift | JSDoc comments between fields omitted from snippet |
| 4 | DreamTask.ts:L23 (DreamPhase) | verbatim | Exact match |
| 5 | DreamTask.ts:L16-L19 (DreamTurn) | drift | Line range wrong (should be L15-L18); content matches |
| 6 | autoDream.ts:L95-L100 (isGateOpen) | verbatim | Exact match |
| 7 | autoDream.ts:L216-L221 (Bash restriction) | drift | Leading blank line and trailing session-list portion omitted; line wrapping differs |
| 8 | autoDream.ts:L56 (SESSION_SCAN_INTERVAL_MS) | verbatim | Exact match |
| 9 | autoDream.ts:L268-L270 (rollback) | drift | Comment on L269 omitted from snippet |

## Factual Claims

All factual claims in the chapter were checked against the source code and found to be accurate:
- Three-gate system description matches the code
- GrowthBook feature flag name `tengu_onyx_plover` is correct
- Lock race-detection mechanism description is accurate
- Four-phase consolidation prompt (Orient, Gather, Consolidate, Prune) matches the source
- `HOLDER_STALE_MS` = 1 hour (60 * 60 * 1000) is correct
- `SESSION_SCAN_INTERVAL_MS` = 10 minutes is correct
- MAX_TURNS = 30 is correct
- Default minHours=24, minSessions=5 is correct

## Uncited Sources

- `src/services/autoDream/config.ts` — not directly cited, though `isAutoDreamEnabled()` is referenced

## Summary

- 9 snippets: 5 verbatim, 4 drift, 0 hallucinated
- 2 bad line-number citations
- 1 uncited source file (informational)
- All factual claims verified
