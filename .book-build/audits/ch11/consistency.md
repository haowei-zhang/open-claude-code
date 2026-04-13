# Consistency Audit: Chapter 11 — Anatomy of a Tool

## Term Conflict Check

All registered terms used in this chapter are used consistently with their canonical definitions:
- "tool" — used consistently as the typed, schema-validated operation
- "deferred tool" — used correctly in shouldDefer/alwaysLoad discussion
- "ToolSearch" — referenced correctly as the progressive tool expansion system
- "PreToolUse" / "PostToolUse" — referenced correctly as hook events
- "observation masking" — referenced correctly in maxResultSizeChars discussion
- "harness" — used correctly as the outer runtime layer

No alternate names for registered terms detected.

## Voice Drift

None detected. Chapter is written in present tense, descriptive, cite-heavy style consistent with house style.

## Proposed New Terms

1. **backfillObservableInput**: A method that injects derived fields (e.g., expanded file paths) into a shallow clone of the tool input for hooks and permission checks, without modifying the original input passed to call().

2. **fail-closed default**: A default value for a safety-critical method that assumes the most restrictive behavior when the method is not explicitly implemented, ensuring that omission cannot lead to unsafe execution.

## Verdict: pass
