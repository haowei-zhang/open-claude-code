# Consistency Audit: Chapter 48

## Term Conflicts

Checked each term in terminology.json against chapter usage:

- "tool": Used correctly throughout (typed, schema-validated operation)
- "subagent": Not used (chapter uses "remote agent" which is appropriate)
- "task": Used correctly (durable unit of work tracked through status transitions)
- "lazySchema": Used correctly (wrapper function that defers Zod schema evaluation)
- "checkpoint-restore": Used correctly (ability to save and resume agent state)
- "feature gate": Used correctly (compile-time boundary via feature())
- "deferred tool": Used correctly (tool not included in initial prompt)
- "fail-closed": Not directly used but the pattern is described accurately

No term conflicts found. All registered terms are used with their canonical definitions.

## Voice Drift

The chapter is written in present tense, descriptive, cite-heavy style consistent with the house style. No voice drift detected.

## Proposed New Terms

1. **remote trigger** - A REST API configuration object on claude.ai CCR that defines a cloud-side agent; invoked via POST /v1/code/triggers/{id}/run to start a remote session.
2. **completion checker** - A type-specific function registered via registerCompletionChecker() that determines remote task completion by checking external state (e.g., PR merge status) on every poll tick.
3. **polling-based event stream** - A pull-based coordination model where the local session periodically fetches remote agent events via HTTP, as opposed to push-based WebSocket or webhook callbacks.
