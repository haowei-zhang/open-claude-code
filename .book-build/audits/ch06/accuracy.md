# Accuracy Audit: Chapter 6 — `main.tsx` and the Command Router

## Summary

Verified all line citations, factual claims, and code snippets against `src/main.tsx` (4,683 lines).

## Citation Verification

### Verified Citations (26/28)
- `src/main.tsx:L585` (main function) — CORRECT: line 585 is `export async function main()`
- `src/main.tsx:L547-L552` (PendingConnect) — DRIFT: type starts at L543, variable at L548; range should be L543-L552
- `src/main.tsx:L517-L540` (initializeEntrypoint) — CORRECT
- `src/main.tsx:L818-L843` (client type) — CORRECT: IIFE starts at L818
- `src/main.tsx:L835-L843` (setQuestionPreviewFormat) — CORRECT
- `src/main.tsx:L266-L271` (debug detection) — CORRECT
- `src/main.tsx:L326-L352` (runMigrations) — CORRECT
- `src/main.tsx:L323` (@[MODEL LAUNCH] marker) — CORRECT
- `src/main.tsx:L349-L351` (async migration) — CORRECT
- `src/main.tsx:L388-L431` (startDeferredPrefetches) — CORRECT
- `src/main.tsx:L360-L380` (prefetchSystemContextIfSafe) — CORRECT
- `src/main.tsx:L502-L516` (eagerLoadSettings) — CORRECT
- `src/main.tsx:L432-L483` (loadSettingsFromFlag) — CORRECT
- `src/main.tsx:L447-L456` (content hash comments) — CORRECT
- `src/main.tsx:L484-L496` (loadSettingSourcesFromFlag) — CORRECT
- `src/main.tsx:L857-L883` (getInputPrompt) — CORRECT
- `src/main.tsx:L884-L900` (run function) — CORRECT
- `src/main.tsx:L602-L604` (SIGINT skip) — CORRECT
- `src/main.tsx:L612-L642` (deep-link URL handling) — CORRECT
- `src/main.tsx:L666-L676` (macOS URL scheme) — CORRECT
- `src/main.tsx:L566-L584` (PendingSSH) — DRIFT: type starts at L567, not L566
- `src/main.tsx:L700-L795` (SSH flag forwarding) — CORRECT
- `src/main.tsx:L739-L759` (extractFlag helper) — CORRECT
- `src/main.tsx:L786-L789` (headless SSH rejection) — CORRECT

## Snippet Verification

### Snippet 1: `src/main.tsx:L585-L607`
- **Verdict**: DRIFT
- The chapter snippet omits the `profileCheckpoint('main_warning_handler_initialized')` line at L607 and the `See:` URL comment at L590. Comments are slightly condensed. Code identifiers match exactly.

### Snippet 2: `src/main.tsx:L547-L552`
- **Verdict**: DRIFT
- Range should be L543-L552 (type definition starts at L543). The chapter cites L547-L552, missing the first line of the type. Content of the included lines matches exactly.

### Snippet 3: `src/main.tsx:L326-L352`
- **Verdict**: DRIFT
- Chapter snippet omits `resetAutoModeOptInForDefaultOffer()` and `migrateFennecToOpus()` which are present in the actual code (L338-342). Comments are abbreviated with `// ...`. Identifiers match.

### Snippet 4: `src/main.tsx:L818-L834`
- **Verdict**: VERBATIM
- Client type IIFE matches character-by-character with the actual source.

## Factual Claims
- "approximately 4,700 lines" — CORRECT (actual: 4,683)
- "profileCheckpoint('main_tsx_entry')" — CORRECT (L12)
- "startMdmRawRead()" — CORRECT (L16)
- "startKeychainPrefetch()" — CORRECT (L20)
- "~65ms sequential cost" — unverifiable from code alone; matches code comments
- All architectural descriptions verified correct

## Uncited Sources
- None (only `src/main.tsx` in the brief)
