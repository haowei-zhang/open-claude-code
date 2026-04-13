# Gaps Audit: Chapter 43

## Verdict: revise

## Brief Coverage

The brief synopsis states: "REPL screen, prompt composer, typeahead (~1,400 LOC hook), voice integration (~1,100 LOC hook), bridge/inbox, keybinding context."

### Overview vs Synopsis Match
The chapter's Overview covers REPL, PromptInput, typeahead, voice, and keybindings. The brief mentions "bridge/inbox" but the chapter does not cover the bridge/inbox subsystem.

### Source File Citation

Brief source files:
- `src/screens/REPL.tsx` -- Cited extensively
- `src/components/PromptInput/PromptInput.tsx` -- Cited with snippet
- `src/hooks/useTypeahead.tsx` -- Cited with multiple snippets
- `src/hooks/useVoice.ts` -- Cited with multiple snippets
- `src/hooks/useGlobalKeybindings.tsx` -- Cited with snippet

All 5 brief source files are cited. No uncited brief files.

### Mandated Diagrams

Brief requires:
- (a) classDiagram of REPL components -- **MISSING**
- (b) sequenceDiagram of a prompt from keystroke to model send -- Present
- (c) stateDiagram-v2 of voice capture -- Present (but duplicated)

### Minimum Counts

- citation_count: 24 (minimum 6) -- PASS
- diagram_count: 5 (minimum 2) -- PASS
- snippet_count: 6 (minimum 4) -- PASS

### Top 3 Source Files Without Snippets

The top 3 source files by centrality are:
1. `src/screens/REPL.tsx` -- Has snippets (L98-103 useVoiceIntegration, L303-305 RECENT_SCROLL_REPIN_WINDOW_MS, L410-437 search index, L473-476 TITLE_ANIMATION, L625 useCommandQueue, L745-768 notification hooks)
2. `src/hooks/useTypeahead.tsx` -- Has snippets (L108-116 UseTypeaheadResult, L52-74 getPreservedSelection, L206-224 shell completion abort, L494-505 cache refresh)
3. `src/hooks/useVoice.ts` -- Has snippets (L143-155 VoiceState, L42-89 LANGUAGE_NAME_TO_CODE, L185-197 computeLevel, L140-141 VoiceModule)

All top 3 files have snippets. No gaps.

### Uncovered Topics

1. **Bridge/inbox**: The brief mentions "bridge/inbox" but the chapter does not cover the REPL's bridge communication system (how messages arrive from VS Code, JetBrains, or web IDE). This is a gap relative to the brief.

2. **PromptInput internal architecture**: The chapter shows the Props type but does not dive into PromptInput's internal state management, cursor handling, or text selection mechanics, which are substantial parts of the ~2,300 LOC component.

3. **MCP integration in REPL**: The REPL manages MCP client connections as part of its state, and the notification hooks monitor MCP connectivity, but the chapter does not explain how the REPL initializes, connects, and routes MCP tools. This is mentioned in the Props section but not explained.

## Summary

- uncited_brief_files: []
- missing_diagrams: [{"required": "classDiagram of REPL components", "found": false}]
- uncovered_topics: ["bridge/inbox communication system", "PromptInput internal architecture", "MCP integration in REPL"]
- citation_count: 24
- diagram_count: 5
- snippet_count: 6
- top_files_without_snippets: []
