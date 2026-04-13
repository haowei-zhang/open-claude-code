# Accuracy Audit Report: Chapter 28

## Summary

Verdict: **revise**

30 citations checked, 27 verified. 30 code snippets examined. 28 verbatim, 2 drift, 0 hallucinated.

## Issues

### 1. Snippet drift: apiMicrocompact.ts ContextEditStrategy (chapter line ~714)

The chapter renders the `ContextEditStrategy` type with compact single-line formatting for nested object types:
```typescript
trigger?: { type: 'input_tokens'; value: number }
```

The actual source code at `src/services/compact/apiMicrocompact.ts:38-52` spreads the same object across multiple lines:
```typescript
trigger?: {
  type: 'input_tokens'
  value: number
}
```

Semantically identical but whitespace differs. This is a drift, not hallucination.

### 2. Referenced file does not exist: snipCompact.ts

The chapter (line 41) mentions `src/services/compact/snipCompact.ts` as a feature-gated file. This file does not exist in the current repository. The snip functionality is referenced in query.ts via `snipModule!.snipCompactIfNeeded` but the underlying module location is unclear.

### 3. Referenced directory does not exist: contextCollapse/

The chapter (line 41) references `src/services/contextCollapse/`. This directory does not exist in the current repository. Context collapse is referenced in autoCompact.ts and postCompactCleanup.ts via `require('../contextCollapse/index.js')`, suggesting it may be feature-gated and only present in internal builds.

## Verified Snippets

All 30 snippets were examined. 28 are verbatim matches with the source code. Key verified snippets include:

- CompactionResult interface (compact.ts:299-310) - exact match
- buildPostCompactMessages (compact.ts:330-338) - exact match
- MicrocompactResult type (microCompact.ts:215-220) - exact match
- PendingCacheEdits type (microCompact.ts:207-213) - exact match
- COMPACTABLE_TOOLS set (microCompact.ts:41-50) - exact match
- microcompactMessages function (microCompact.ts:253-293) - exact match with trim
- shouldAutoCompact recursion guards (autoCompact.ts:170-173) - exact match
- getAutoCompactThreshold (autoCompact.ts:72-91) - exact match
- getEffectiveContextWindowSize (autoCompact.ts:33-49) - exact match
- trySessionMemoryCompaction call (autoCompact.ts:288-310) - exact match
- NO_TOOLS_PREAMBLE (prompt.ts:19-24) - exact match
- ERROR_MESSAGE_PROMPT_TOO_LONG (compact.ts:293-294) - exact match
- truncateHeadForPTLRetry (compact.ts:243-291) - exact match with trim
- annotateBoundaryWithPreservedSegment (compact.ts:349-367) - exact match
- partialCompactConversation signature (compact.ts:772-779) - exact match

## Uncited Sources

The following files in the source_files list were not explicitly cited:
- compactWarningHook.ts
- compactWarningState.ts
- grouping.ts
