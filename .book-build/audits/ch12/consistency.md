# Consistency Audit Report for Chapter 12: Tool Dispatch Pipeline

## Term Verification

### Registered Terms Used in This Chapter
- **tool** — used correctly with canonical definition (typed, schema-validated operation via tool dispatch pipeline)
- **hook** — used correctly (deterministic lifecycle event handler)
- **PreToolUse** — used correctly (hook event fired before tool's call() method)
- **PostToolUse** — used correctly (hook event fired after tool's call() method)
- **deferred tool** — used correctly (tool not included in initial prompt, loaded on demand)
- **ToolSearch** — used correctly (progressive tool expansion system)
- **permission mode** — used correctly (named configuration governing tool-access behavior)
- **classifier** — used correctly (function mapping shell command to risk band)
- **query loop** — used correctly (async generator function in src/query.ts)
- **harness** — used correctly (outer runtime layer orchestrating model interactions)
- **structural control** — used correctly (architectural constraint making failure modes impossible)

### Term Conflicts
None detected. All registered terms are used with their canonical definitions.

### Alternate Names Check
- "tool call" is used alongside "tool use" — both are acceptable as the chapter uses "tool call" for the model's intent and "tool use" for the API block type. No conflict with the registry.
- "read-only" and "concurrency-safe" are used interchangeably in context — the chapter explains they are equivalent, which is consistent with the codebase.

### Voice Drift
No voice drift detected. The chapter maintains present tense, descriptive, cite-heavy register consistent with house style.

### Proposed New Terms
1. **dispatch pipeline** — The ordered sequence of stages a tool invocation passes through (validation → permission → execution → post-hooks), from model intent to result.
2. **sibling abort** — The mechanism by which a Bash tool error cancels sibling concurrent tool executions via a child abort controller, without aborting the parent query.
3. **speculative classifier check** — A pattern where a bash classifier result is raced against a timeout during the permission decision flow, allowing auto-approval of common commands without showing a dialog.
