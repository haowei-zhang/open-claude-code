# Consistency Audit - Chapter 3: A Guided Tour of the Repository

## Term Conflicts
- Count: 0
- The chapter correctly uses all registered terms (harness, tool, compaction, memory, session, memdir, etc.)
- The chapter discusses the "hooks" naming collision between React hooks and harness hooks as a known issue in the codebase, which is appropriate

## Voice Drift
- None detected. The chapter maintains the house style (present tense, descriptive, cite-heavy).

## Proposed New Terms
1. "directory contract" - The consistent subdirectory pattern each tool follows: a main implementation file, a prompt.ts for model description, and optionally a UI.tsx for terminal rendering. (Already registered in terminology.json)
2. "feature gate" - A compile-time boundary checked via feature() from bun:bundle that determines which code exists in the external build. (Already registered in terminology.json)
3. "lazy require" - A pattern using require() at call time instead of import time to break circular dependencies between modules. (Already registered in terminology.json)

## Notes
- All three proposed terms from previous audit rounds have been added to terminology.json
- No new terms to propose this round

## Verdict: pass
