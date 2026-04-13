# Consistency Audit - Chapter 14: The Bash Tool, Classifiers, and Sandboxing

## Terminology Consistency Check

### Registered Terms Used
- "classifier" - Used consistently with registry definition ("A function that maps a shell command string to a risk band")
- "permission mode" - Used consistently with registry definition
- "dispatch pipeline" - Used in context of tool dispatch
- "PreToolUse" / "PostToolUse" - Referenced correctly as hook events
- "checkpoint-restore" - Used consistently
- "fail-closed default" - Concept referenced correctly
- "sandbox" - Used consistently throughout
- "harness" - Used correctly per registry definition

### Term Conflicts
1. "tool call" used once instead of registry's canonical "tool use" - MILD conflict
   - Chapter line ~107: "the request flows through a multi-stage pipeline"
   - Context: "When the model requests a Bash tool call" - should be "tool use"

### Voice Drift
- Chapter maintains present tense, descriptive, cite-heavy style throughout
- No voice drift detected
- Deep-dive sections at end use same register as main body

### Proposed New Terms
1. "flag-level allowlisting" - The practice of validating individual CLI flags for complex commands rather than using command-level regexes, implemented via COMMAND_ALLOWLIST with typed FlagArgType entries
2. "parser differential" - A security vulnerability where the validator and the shell interpreter disagree on the parsing of a command string, enabling bypass attacks (e.g., xargs -i vs -I)
3. "fixed-point command stripping" - The iterative approach to stripping env vars and wrapper commands from shell invocations, applying both operations until no new candidates are produced

## Issues
1 term conflict (tool call vs tool use), no voice drift
