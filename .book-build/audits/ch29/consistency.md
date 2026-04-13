# Consistency Audit: Chapter 29 - Session Persistence and Resume

## Terminology Check

### Registered terms used in this chapter:
- "session" - used consistently with registry definition ("A single invocation of the agent from startup to shutdown, persisted as JSONL entries with tombstones for resumability") - PASS
- "compaction" - used consistently with registry definition - PASS
- "checkpoint-restore" - used consistently with registry definition - PASS
- "hook" - used correctly in context of cleanup handler - PASS
- "worktree" - used consistently - PASS
- "observation masking" - not used in this chapter - N/A
- "subagent" - used correctly - PASS
- "tool use" / "tool call" - the chapter uses "tool uses" which matches the registry preference - PASS
- "back-pressure" - not used directly - N/A
- "query loop" - mentioned in context - PASS

### Potential conflicts:
- Chapter uses "tombstone" informally but this is not a registered term - not a conflict
- Chapter uses "sidechain" which is not in the registry but is used consistently throughout the codebase

### Voice drift check:
- Present tense throughout: YES
- Descriptive style: YES
- Cite-heavy: YES (57 citations)
- No voice drift detected

### Proposed new terms:
1. "append-only discipline" - The design principle that no entry is ever modified after write; deletions are represented by omission or explicit removal. Core to cc's session persistence model.
2. "progress bridge" - The map that chains through consecutive legacy progress entries and rewrites subsequent messages' parentUuid to skip past them, maintaining chain integrity across entries no longer in the type union.
3. "head/tail read" - The optimization pattern of reading only the first and last 64KB of a JSONL file to extract metadata (first prompt, last title, last tag) without parsing the full content.
4. "session stamping" - The practice of re-stamping provenance fields (sessionId, cwd, entrypoint, version) after message spreads to prevent cross-session contamination in forked or resumed sessions.
5. "metadata re-append" - The pattern of unconditionally re-writing session metadata entries near EOF after compaction or on exit to keep them within the 64KB tail window for fast reads.
