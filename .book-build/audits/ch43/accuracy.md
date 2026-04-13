# Accuracy Audit: Chapter 43

## Verdict: revise

## Issues Found

### Wrong line number citations

1. **L13**: `src/screens/REPL.tsx:L11` claims Screen type is at line 11. Actual location is L571 (`export type Screen = 'prompt' | 'transcript'`). Line 11 is a `useInput` import.

2. **L36**: `src/components/PromptInput/PromptInput.tsx:L81-L107` claims Props type is at lines 81-107. Actual Props type starts at L124. Lines 81-107 contain import statements.

### Snippet verification

- `useTypeahead.tsx:L108-L116` (UseTypeaheadResult): **verbatim** - matches exactly
- `useVoice.ts:L143-L155` (VoiceState/UseVoiceOptions/UseVoiceReturn): **verbatim** - matches exactly
- `useVoice.ts:L42-L89` (LANGUAGE_NAME_TO_CODE): **drift** - chapter omits several entries present in actual file (espanol, francais, portugues, etc.) and abbreviates with `// ... 20 total languages`. The shown entries match but the line range is approximate.
- `useGlobalKeybindings.tsx:L36-L46` (GlobalKeybindingHandlers): **verbatim** - matches exactly
- `useVoice.ts:L185-L197` (computeLevel): **drift** - actual file has comments (`// 16-bit = 2 bytes per sample`, `// Read 16-bit signed little-endian`) that are missing from the chapter snippet
- `useVoice.ts:L140-L141` (VoiceModule): **verbatim** - matches exactly

### Duplicate content sections

The chapter contains significant duplicated content:
- "Typeahead suggestion pipeline" (L185) and "Typeahead suggestion pipeline (continued)" (L229) contain nearly identical mermaid flowcharts and repetitive prose about `generateCommandSuggestions`, `applyCommandSuggestion`, and `commandArgumentHint`.
- The voice section's `focusMode` explanation, `computeLevel` code snippet, and `RELEASE_TIMEOUT_MS`/`FIRST_PRESS_FALLBACK_MS` discussion appear twice (L282-314 and L351-371).

This is not strictly an "accuracy" issue but indicates the chapter was not properly edited.

### Factual accuracy

All factual claims about source code behavior appear accurate. The described architectures of REPL, PromptInput, useTypeahead, useVoice, and useGlobalKeybindings match the actual implementations.

### Uncited sources

- `src/components/Messages.tsx` - mentioned in prose but no snippet
- `src/query.ts` - referenced but no snippet

## Summary

- citation_total: 24
- citation_verified: 22 (2 wrong line numbers)
- snippet_total: 18 (including 2 duplicate code blocks)
- snippet_verbatim: 14
- snippet_drift: 2
- snippet_hallucinated: 0
