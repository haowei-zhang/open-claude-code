# Consistency Audit: Chapter 52

## Term Conflicts

No term conflicts found. All registered terms used with canonical definitions:
- "tool" — used correctly
- "permission mode" — used correctly
- "classifier" — used correctly
- "hook" — used correctly
- "checkpoint-restore" — used correctly
- "SSRF guard" — used correctly
- "fail-closed" — used correctly
- "denial tracking" — used correctly
- "YOLO classifier" — used correctly
- "structural control" — used correctly

## Voice Drift

None detected. Chapter maintains present tense, descriptive, cite-heavy register consistent with house style.

## Proposed New Terms

1. **five-layer defense-in-depth**: HER Section 12.1's security model consisting of prompt-level guardrails, schema-level restrictions, runtime approval, tool-level validation, and lifecycle hooks, each acting as a progressive filter on tool invocations.

2. **circuit-broken auto mode**: A state in which auto mode has been permanently disabled for the current session after a server-side kill switch fires, preventing re-entry even on subsequent GrowthBook refreshes.

3. **session token unlink**: The security pattern of deleting a secret file from disk after the infrastructure that depends on it has been confirmed running, ensuring the secret is available for retry during init but invisible to the agent loop during operation.

## Verdict: pass
