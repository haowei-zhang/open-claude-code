# Accuracy Audit: Chapter 30 — AppState: The Redux-like Store

## Summary

The chapter accurately describes the dual-state architecture of cc's state management. Most citations are correct and the code snippets from `AppStateStore.ts`, `store.ts`, `onChangeAppState.ts`, and `bootstrap/state.ts` are verbatim. However, there are significant issues with snippets from `AppState.tsx`, which is compiled by the React compiler and the source file on disk contains minified/compiled output rather than the clean TypeScript shown in the chapter.

## Citation Verification

### Verified citations
- `src/state/AppStateStore.ts:L89-L158` — AppState type starts at line 89. Accurate.
- `src/state/AppStateStore.ts:L159-L167` — Intersection type with tasks/Map. Accurate.
- `src/state/AppStateStore.ts:L323-L345` — teamContext definition. Accurate.
- `src/state/AppStateStore.ts:L59-L77` — SpeculationState. Close (actual is L58-L77).
- `src/state/AppStateStore.ts:L173-L184` — mcp field. Accurate.
- `src/state/AppStateStore.ts:L459-L467` — lazy require for teammate. Accurate.
- `src/state/store.ts:L1-L34` — entire store implementation. Accurate.
- `src/bootstrap/state.ts:L45-L257` — State type. Accurate.
- `src/bootstrap/state.ts:L31` — DO NOT ADD MORE STATE HERE comment. Accurate.
- `src/bootstrap/state.ts:L429` — STATE singleton. Accurate.
- `src/bootstrap/state.ts:L468-L479` — switchSession. Accurate.
- `src/bootstrap/state.ts:L557-L564` — addToTotalCostState. Accurate.
- `src/bootstrap/state.ts:L919-L921` — resetStateForTests guard. Accurate.
- `src/bootstrap/state.ts:L923-L929` — reset implementation. Accurate.
- `src/state/onChangeAppState.ts:L65-L92` — mode diff. Accurate.
- `src/state/onChangeAppState.ts:L156-L170` — cache invalidation. Accurate.
- `src/state/onChangeAppState.ts:L24-L41` — externalMetadataToAppState. Accurate.

### Problematic citations
- `src/state/AppState.tsx:L49-L56` — The file on disk is React compiler output; the clean code shown in the chapter corresponds to the source-mapped original, not the file at those lines.
- `src/state/AppState.tsx:L142-L163` — Same issue: compiled output on disk differs from clean code shown.
- `src/state/AppState.tsx:L170-L172` — Same issue.
- `src/state/AppState.tsx:L84-L91` — Same issue.
- `src/state/AppState.tsx:L44-L48` — Same issue.
- `src/state/AppState.tsx:L62-L70` — Same issue.
- `src/state/AppState.tsx:L186-L199` — Same issue.
- `src/state/AppState.tsx:L148-L153` — The chapter shows `if (false && state === selected)` but the actual source-mapped code has `if ("external" === "ant" && state === selected)`. The `false &&` is the compiled/external-build version where the ant-only check is dead-code-eliminated.

## Snippet Verification

- 21 total snippets
- 14 verbatim (from AppStateStore.ts, store.ts, onChangeAppState.ts, bootstrap/state.ts)
- 7 drift (all from AppState.tsx — compiled output differs from clean source shown)
- 0 hallucinated (the `false &&` vs `"external" === "ant"` is drift, not hallucination — the logic is equivalent in the external build)

## Factual Claims

- "approximately 450 lines of type declarations" — AppState spans L89-L452, which is 363 lines. "Approximately 450" is a slight overcount but acceptable.
- "over 250 fields" for bootstrap State — The type declaration spans ~212 lines, and there are far fewer than 250 individual fields. This is inaccurate; the count is closer to ~80-100 fields.
- "34-line function in src/state/store.ts" — Accurate (exactly 34 lines).
- All claims about onChangeAppState behavior verified against source code.
