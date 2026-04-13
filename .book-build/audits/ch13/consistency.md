# Consistency Audit: Chapter 13 — File System Tools

## Term Conflicts
None found. All registered terms used correctly:
- "tool" — used consistently with canonical definition
- "dispatch pipeline" — used correctly
- "permission mode" — not used in this chapter
- "hook" — used correctly (PreToolUse, PostToolUse)
- "compaction" — not used in this chapter
- "directory contract" — implicitly referenced but not by name

## Voice Drift
None detected. The chapter maintains present tense, descriptive, cite-heavy register consistent with house style.

## Proposed New Terms
1. **readFileState** — A Map<string, ReadFileEntry> maintained in ToolUseContext that records every file the model has read, along with content, modification timestamp, offset, and limit; used to enforce the read-before-write contract and read deduplication.
2. **read-before-write contract** — The invariant enforced by Write, Edit, and NotebookEdit tools that the model must have read a file before modifying it, preventing blind overwrites and silent data loss.
3. **file-history backup** — A content-hash-keyed backup created before file edits, stored at ~/.claude/file-history/{sessionId}/{sha256-hash}@v{N}, providing checkpoint-restore semantics for undo operations.

## Verdict: pass
