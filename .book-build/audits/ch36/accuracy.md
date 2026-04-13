# Accuracy Audit: Chapter 36 - The Hook Schema and Lifecycle Events

## Citation Verification

### Source files checked
- `src/schemas/hooks.ts` (222 LOC) - cited extensively, verified
- `src/utils/hooks/hooksSettings.ts` (271 LOC) - cited extensively, verified
- `src/utils/hooks/hooksConfigManager.ts` (400 LOC) - cited extensively, verified
- `src/utils/hooks.ts` (5022 LOC) - in brief but NOT cited by chapter

### Code Snippet Verification

| # | Snippet | Claimed Lines | Actual Lines | Verdict |
|---|---------|---------------|--------------|---------|
| 1 | BashCommandHookSchema | L31-L65 | L32-L65 | drift (line offset by 1, describe text differs: shell describe says "bash/zsh/sh" in actual, omitted in chapter; asyncRewake describe says "blocking error" in actual, chapter paraphrases) |
| 2 | PromptHookSchema | L67-L95 | L67-L95 | drift (model describe differs slightly; chapter omits $ARGUMENTS note from agent hook) |
| 3 | HttpHookSchema | L97-L126 | L97-L126 | drift (headers describe truncated; allowedEnvVars describe truncated, missing "Required for env var interpolation to work") |
| 4 | AgentHookSchema | L128-L163 | L128-L163 | drift (comment truncated; prompt describe truncated - missing "$ARGUMENTS" and example; model describe differs - actual says "Haiku" default, chapter says "default small fast model") |
| 5 | IfConditionSchema | L19-L27 | L19-L27 | verbatim |
| 6 | HookMatcherSchema + HooksSchema | L194-L205 | L194-L213 | drift (HooksSchema truncated, missing comment) |
| 7 | IndividualHookConfig | L22-L28 | L22-L28 | verbatim |
| 8 | getAllHooks | L92-L161 | L92-L161 | drift (minor formatting differences, missing `getSettingsForSource` details) |
| 9 | getHookEventMetadata memoize | L26-L27 | L26-L27 | drift (function body abbreviated with `...`) |
| 10 | PreToolUse metadata | L29-L37 | L29-L37 | verbatim |
| 11 | PostToolUse metadata | L38-L47 | L38-L46 | drift (line count off by 1) |
| 12 | Stop metadata | L95-L99 | L95-L99 | verbatim |
| 13 | UserPromptSubmit metadata | L83-L86 | L81-L85 | drift (line numbers off by 2) |
| 14 | CwdChanged metadata | L258-L263 | L254-L258 | drift (line numbers off) |
| 15 | isHookEqual | L33-L65 | L33-L65 | drift (minor formatting) |
| 16 | getSourcePriority | L256-L259 | L256-L259 | verbatim |
| 17 | groupHooksByEventAndMatcher ANT-ONLY | L346-L359 | L346-L359 | drift (minor formatting) |
| 18 | PermissionDenied metadata | L56-L64 | L56-L64 | verbatim |
| 19 | ConfigChange metadata | L214-L218 | L214-L218 | verbatim |
| 20 | InstructionsLoaded metadata | L229-L232 | L229-L233 | drift (truncated description) |

### Factual Claims
- hooks.ts 222 LOC: actual 222 - correct
- hooksSettings.ts 271 LOC: actual 271 - correct
- hooksConfigManager.ts 400 LOC: actual 400 - correct
- "23 distinct lifecycle events": Counted from source = 25 events (PreToolUse, PostToolUse, PostToolUseFailure, PermissionDenied, Notification, UserPromptSubmit, SessionStart, SessionEnd, Stop, StopFailure, SubagentStart, SubagentStop, PreCompact, PostCompact, PermissionRequest, Setup, TeammateIdle, TaskCreated, TaskCompleted, Elicitation, ElicitationResult, ConfigChange, WorktreeCreate, WorktreeRemove, InstructionsLoaded, CwdChanged, FileChanged) - actually 27 events. Chapter claims 23 - **unsupported claim**.
- "five hook types (command, prompt, HTTP, agent, function)": The schemas define 4 persistable types; function is programmatic only. Chapter correctly notes this in the text but the initial claim of "five" is slightly misleading since HookCommand only has 4.

### Uncited Sources
- `src/utils/hooks.ts` (5022 LOC) - the main hook execution runtime. This is a significant file that the chapter should cite.
