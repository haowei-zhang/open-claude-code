# Gaps Audit Report — Chapter 27

## Brief Coverage Check

### Synopsis Match
The brief synopsis: "services/SessionMemory/ and memory extraction pipelines, prompts that drive it, feature gates."
- SessionMemory directory: COVERED
- Memory extraction pipeline: COVERED (extraction lifecycle, trigger logic, forked agent)
- Prompts that drive it: COVERED (getDefaultUpdatePrompt, buildSessionMemoryUpdatePrompt, generateSectionReminders)
- Feature gates: PARTIALLY COVERED (tengu_session_memory gate mentioned, but GrowthBook integration not deeply explored)

### Source File Coverage

Required source files:
1. `src/services/SessionMemory/sessionMemory.ts` — CITED multiple times
2. `src/services/SessionMemory/prompts.ts` — CITED multiple times

Both source files are cited. No uncited brief files.

However, the chapter also references `src/services/compact/sessionMemoryCompact.ts` which is NOT in the brief's source_files list. This is fine — additional sources are allowed.

### Diagram Requirements

1. (a) sequenceDiagram of memory extraction after a session — **PRESENT** (extraction lifecycle diagram)
2. (b) flowchart of memory promotion session -> memdir — **MISSING**

The chapter has a flowchart about section size enforcement and truncation flow, but NOT about the memory promotion pipeline from session to memdir. This is a gap.

### Minimum Counts

- Citation count: 15 (>= 6, PASS)
- Diagram count: 2 (>= 2, PASS)
- Snippet count: 15 (>= 4, PASS)

### Top Source Files Without Snippets

Both top source files have snippets:
- `src/services/SessionMemory/sessionMemory.ts`: Multiple snippets
- `src/services/SessionMemory/prompts.ts`: Multiple snippets

### Uncovered Topics

1. **Memory promotion pipeline (session -> memdir)**: The brief's diagram requirement (b) explicitly asks for this flow. The chapter does not cover how session memory notes get promoted or transferred to the persistent memdir system. This is a significant gap.
2. **Feature gate details**: The `tengu_session_memory` feature gate is mentioned but the GrowthBook integration and remote configuration loading are not explored in depth.
3. **sessionMemoryUtils.ts**: The chapter references utility functions (shouldExtractMemory, markExtractionStarted, etc.) from this module but does not cite or discuss the file. While it's not in the brief's source_files, it's an important supporting module.

## Verdict: revise (1 missing required diagram, 1-3 uncovered topics)
