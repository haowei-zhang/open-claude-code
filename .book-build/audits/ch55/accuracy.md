# Accuracy Audit — Chapter 55

## Citation Verification

### Snippet 1: `src/utils/tasks.ts:L76-L89` — Task schema definition
- File exists: YES
- Line range valid: YES
- Content match: The chapter shows `z.object({...})` with fields id, subject, description, activeForm, owner, status, blocks, blockedBy, metadata. The actual source at L76-L89 matches exactly (verbatim). The `activeForm` comment in source is "// present continuous form for spinner (e.g., \"Running tests\")" which the chapter omitted. The `owner` comment "// agent ID" was also omitted. These are comment omissions, not content drift.
- Verdict: **verbatim** (comment trimming is standard)

### Snippet 2: `src/utils/tasks.ts:L69-L74` — Task statuses as a const tuple
- File exists: YES
- Line range valid: YES
- Content match: The chapter shows TASK_STATUSES, TaskStatusSchema, TaskStatus. Actual source at L69-L74 matches. Chapter omits the comment on activeForm. Otherwise exact.
- Verdict: **verbatim**

### Snippet 3: `src/services/compact/autoCompact.ts:L51-L70` — Autocompact tracking and thresholds
- File exists: YES
- Line range valid: YES
- Content match: Chapter shows AutoCompactTrackingState type, four const values, and MAX_CONSECUTIVE_AUTOCOMPACT_FAILURES. The actual source includes comments between the type and the constants that the chapter omits (e.g., "// Unique ID per turn", "// Consecutive autocompact failures..."). The constants themselves match. The chapter adds `turnCounter: number` and `turnId: string` which match source. The `consecutiveFailures?: number` matches.
- Verdict: **verbatim** (with standard comment trimming)

### Snippet 4: `src/memdir/memdir.ts:L57-L80` — Entrypoint truncation with dual caps
- File exists: YES
- Line range valid: YES
- Content match: Chapter shows truncateEntrypointContent function. Actual source at L57-L80 matches closely. The chapter shows `wasLineTruncated` and `wasByteTruncated` checks, and the early return and truncation logic. One difference: the actual source at L64 has a comment about "long lines are the failure mode the byte cap targets" which the chapter omits. The actual source returns an object with more fields (wasLineTruncated, wasByteTruncated) in the early return — the chapter shows `{ content: trimmed, lineCount, byteCount, wasLineTruncated, wasByteTruncated }` which matches the actual source.
- Verdict: **verbatim** (with comment trimming)

### Snippet 5: `src/utils/hooks.ts:L67-L109` — Hook event type definitions
- File exists: YES
- Line range valid: YES (L67-L109 is 43 lines)
- Content match: The chapter shows `import type { PreToolUseHookInput, PostToolUseHookInput, ... }` as the content. However, the actual source at L67-L109 is import statements, not the type definitions the caption suggests. The actual code imports these types from `../types/hooks.js` and `src/entrypoints/agentSdkTypes.js`. The chapter's snippet correctly lists many of the hook input types (PreToolUseHookInput, PostToolUseHookInput, PreCompactHookInput, etc.) but the source is import statements, not type definitions.
- Problem: The caption says "Hook event type definitions" but the actual code is import statements. The listed types DO appear in the import list at those lines, but the characterization as "type definitions" is misleading. The types are defined elsewhere.
- Additionally, the chapter's snippet shows an ellipsis `...` at the end, and the actual import list includes many more types (HookEvent, HookInput, NotificationHookInput, PermissionDeniedHookInput, StopFailureHookInput, TeammateIdleHookInput, ConfigChangeHookInput, CwdChangedHookInput, FileChangedHookInput, InstructionsLoadedHookInput, UserPromptSubmitHookInput, PermissionRequestHookInput, ElicitationHookInput, ElicitationResultHookInput, PermissionUpdate, ExitReason, etc.) that the chapter omits.
- Verdict: **drift** (caption is inaccurate — these are imports, not definitions; the listed names match but are selectively shown)

### Snippet 6: `src/services/tools/toolHooks.ts:L39-L55` — Post-tool-use hook iteration
- File exists: YES
- Line range valid: YES
- Content match: Chapter shows `runPostToolUseHooks` async generator function with matching signature (toolUseContext, tool, toolUseID, messageId, toolInput, toolResponse, requestId, mcpServerType, mcpServerBaseUrl). The actual source matches exactly at L39-L55. The function body (postToolStartTime, try block, appState, permissionMode, toolOutput, for await loop) matches.
- Verdict: **verbatim**

### Snippet 7: `src/utils/permissions/PermissionMode.ts:L34-L41` — Mode configuration shape
- File exists: YES
- Line range valid: YES
- Content match: Chapter shows `type PermissionModeConfig = { title, shortTitle, symbol, color, external }`. Actual source at L34-L41 matches exactly.
- Verdict: **verbatim**

### Snippet 8: `src/bootstrap/state.ts:L46-L67` — State shape includes cost and telemetry
- File exists: YES
- Line range valid: YES (L46-L67 is 22 lines)
- Content match: The chapter shows `type State = { totalCostUSD, totalAPIDuration, ... meter, sessionCounter, costCounter, tokenCounter }`. The actual source at L46-L67 shows a `State` type (actually just a plain object shape within the module) with fields totalCostUSD, totalAPIDuration, totalAPIDurationWithoutRetries, totalToolDuration, turnHookDurationMs, turnToolDurationMs, turnToolCount, turnHookCount, but it does NOT have turnClassifierDurationMs or turnClassifierCount shown in the actual source. The chapter omits some fields and adds others. The chapter includes `meter: Meter | null`, `sessionCounter: AttributedCounter | null`, `costCounter: AttributedCounter | null`, `tokenCounter: AttributedCounter | null` — these fields are NOT at L46-L67 in the actual source (they may exist elsewhere in the State type). The actual source at those lines shows different fields including projectRoot, cwd, modelUsage, etc.
- Problem: The snippet claims L46-L67 but the actual content at those lines includes fields like `originalCwd`, `projectRoot` that the chapter doesn't show, and the chapter shows `meter`, `sessionCounter`, `costCounter`, `tokenCounter` that don't appear at those line numbers.
- Verdict: **drift** (the fields shown don't all match the actual line range)

### Snippet 9: `src/cost-tracker.ts:L278-L301` — Per-session cost accumulation with OTel counters
- File exists: YES
- Line range valid: YES
- Content match: Chapter shows `addToTotalSessionCost` function. Actual source at L278-L301 matches closely. The function signature, modelUsage calculation, attrs construction, and the five getCostCounter/getTokenCounter calls all match.
- Verdict: **verbatim**

### Snippet 10: `src/coordinator/coordinatorMode.ts:L36-L41` — Coordinator mode detection
- File exists: YES
- Line range valid: YES
- Content match: Chapter shows `isCoordinatorMode()` function with feature gate check. Actual source at L36-L41 matches exactly.
- Verdict: **verbatim**

## Uncited Source Files
Chapter 55 is a meta-chapter with no source_files in its brief. However, it cites many files. No uncited brief files.

## Summary
- Total citations: 10 code snippets + 3 mermaid diagrams
- snippet_total: 10
- snippet_verbatim: 8 (snippets 1,2,3,4,6,7,9,10)
- snippet_drift: 2 (snippets 5 and 8)
- snippet_hallucinated: 0
- Issues: 2 snippet_drift issues
