# Accuracy Audit — Chapter 17

## Summary

Verified all 11 code snippets and all file/line citations against the actual source code.

## Snippet Verification

| # | Citation | Verdict | Notes |
|---|----------|---------|-------|
| 1 | TodoWriteTool.ts:L13-L17 | verbatim | inputSchema matches exactly |
| 2 | TodoWriteTool.ts:L20-L26 | verbatim | outputSchema matches exactly |
| 3 | AskUserQuestionTool.tsx:L14-L24 | drift | Description strings abbreviated/shortened vs actual; structure and identifiers match |
| 4 | BriefTool.ts:L22-L37 | drift | Description strings truncated (e.g., missing "Supports markdown formatting."); structure matches |
| 5 | ConfigTool.ts:L36-L62 | drift | Description strings truncated; structure and types match |
| 6 | SendMessageTool.ts:L46-L65 | verbatim | StructuredMessage schema matches exactly |
| 7 | TodoWriteTool.ts:L65-L103 | verbatim | call() function body matches exactly |
| 8 | AskUserQuestionTool.tsx:L182-L188 | verbatim | checkPermissions matches exactly |
| 9 | AskUserQuestionTool.tsx:L135-L145 | verbatim | isEnabled() matches exactly |
| 10 | BriefTool.ts:L126-L134 | verbatim | isBriefEnabled() matches exactly |
| 11 | ConfigTool.ts:L98-L107 | verbatim | checkPermissions matches exactly |

## Citation Verification

All source files exist. Line numbers are valid. All factual claims are supported by the source code.

## Uncited Source Files

The following source files from the brief were not explicitly cited with line references:
- src/tools/TodoWriteTool/constants.ts
- src/tools/TodoWriteTool/UI.tsx (not listed in brief but referenced by directory structure)
- src/tools/BriefTool/UI.tsx
- src/tools/ConfigTool/UI.tsx
- src/tools/SendMessageTool/UI.tsx
- src/tools/SendMessageTool/constants.ts

These are informational only; the chapter covers the substantive files adequately.
