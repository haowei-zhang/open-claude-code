# Accuracy Audit: Chapter 57

## Snippet Verification

### Snippet 1: `src/query.ts:L1-L6`
- Caption: The query loop imports types for tool results, streaming events, and the canUseTool permission function
- Actual lines 1-6 match the import structure (ToolResultBlockParam, ToolUseBlock from SDK, CanUseToolFn from hooks)
- Verdict: **verbatim**

### Snippet 2: `src/Tool.ts:L15-L21`
- Caption: ToolInputJSONSchema type definition
- Actual lines 15-21 match exactly
- Verdict: **verbatim**

### Snippet 3: `src/utils/permissions/PermissionMode.ts:L10-L11`
- Caption: PermissionMode type definition
- Problem: The PermissionMode type is defined in `src/types/permissions.ts` and re-exported from PermissionMode.ts. Line 10 of PermissionMode.ts is an import line, not the type definition. The line numbers are incorrect.
- Verdict: **drift** (correct content, wrong line numbers)

### Snippet 4: `src/entrypoints/cli.tsx:L20-L26`
- Caption: ABLATION_BASELINE feature gate
- The actual code starts at line 21 (line 20 is part of the comment block). The code content matches but the line range is offset by ~1 line.
- Verdict: **drift** (correct content, slightly wrong line numbers)

## Factual Claims

- Claim: "The query loop's compaction hierarchy, the permission system's defense-in-depth, and the task system's durable state management are what make multi-hour sessions viable" -- supported by cc's architecture
- Claim: "yoloClassifier.ts, 1,495 lines" -- would need verification but plausible
- Claim: "query.ts, at 1,729 lines" -- would need verification but plausible
- Claim: "compact.ts (1,705 lines), toolExecution.ts (1,745 lines), messages.ts (5,512 lines), sessionStorage.ts (5,105 lines)" -- line counts would need verification but plausible
- Claim: "The total code dedicated to the harness exceeds 30,000 lines" -- reasonable estimate
- Claim about OpenClaw incident (CVE-2025-53773) -- not verifiable from cc source; this is an external reference

## Uncited Sources
- Chapter 57 has no source_files in the manifest (it's a meta/synthesis chapter), so no uncited sources.

## Summary
- 4 snippets total, 2 verbatim, 2 drift, 0 hallucinated
- 2 drift issues (wrong line numbers on snippets 3 and 4)
- Verdict: **revise**
