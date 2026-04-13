# Glossary

**ablation baseline** -- A testing mode enabled by the ABLATION_BASELINE feature flag that disables thinking, compaction, auto-memory, and background tasks for controlled comparison experiments. This baseline isolates the model's raw performance from harness-level interventions, enabling researchers to measure the marginal contribution of each subsystem. (src/entrypoints/cli.tsx)

**agent definition** -- A typed configuration object (built-in, custom, or plugin variant) that specifies an agent's tools, permissions, model, memory scope, and system prompt, discovered from multiple sources with layered precedence. Agent definitions are the declarative configuration surface that controls how subagents behave when spawned by the parent. (src/tools/AgentTool/)

**agent discovery** -- The process of loading agent definitions from multiple sources (built-in, plugin, user, project, flag, policy) with later sources overriding earlier ones for the same agentType. Discovery implements the HER principle of layered configuration, where more specific sources take precedence over general ones. (src/tools/AgentTool/loadAgentsDir.ts)

**agent name registry** -- A Map in AppState that maps agent names to their task IDs, enabling SendMessageTool to address teammates by human-readable name rather than by task ID. The name registry provides an ergonomic abstraction over the raw task ID namespace. (src/state/AppState.tsx)

**analytics sink** -- The backend interface (AnalyticsSink) that receives queued analytics events after attachment, implementing logEvent and logEventAsync methods for fanout to Datadog and first-party logging. The sink abstraction decouples event generation from event delivery, enabling buffering during startup. (src/services/analytics/)

**async rewake** -- A flag on command hooks that causes the hook to run in the background and wake the model if it exits with code 2, enabling blocking-error detection for long-running background tasks. Async rewake bridges the gap between asynchronous hook execution and synchronous model response expectations. (src/schemas/hooks.ts)

**auto-resume on send** -- The pattern where SendMessage attempts resumeAgentBackground() when targeting a stopped agent, restarting it with the incoming message as prompt. Auto-resume ensures that messages sent to idle teammates are not lost, instead triggering the teammate to resume processing. (src/tools/SendMessageTool/)

**backfillObservableInput** -- A method that injects derived fields (e.g., expanded file paths) into a shallow clone of the tool input for hooks and permission checks, without modifying the original input passed to call(). Backfilling ensures that hooks and permission checks see resolved paths while the tool receives the raw input from the model. (src/services/tools/)

**batched connection processing** -- The startup pattern of connecting to MCP servers in parallel groups partitioned by transport type (local vs remote) with different concurrency limits. Batched processing prevents slow remote servers from blocking fast local server connections, optimizing startup time. (src/services/mcp/)

**beta header latching** -- The pattern where an API beta header stays enabled for the rest of a session once activated, preventing prompt cache invalidation from feature toggles. Beta header latching is a cache optimization that avoids the cost of cache breaks caused by mid-session feature flag changes. (src/services/api/claude.ts)

**bridge safety predicate** -- The isBridgeSafeCommand function that classifies commands into three tiers (prompt=safe, local-jsx=blocked, local=allowlist) for remote execution authorization. The safety predicate prevents remote clients from executing arbitrary code through the bridge transport. (src/bridge/)

**BufferedWriter** -- A write abstraction supporting immediate (synchronous) and buffered (asynchronous with periodic flush) modes, used by the debug logging system to balance crash safety against performance. BufferedWriter enables the debug system to switch between guaranteed-crash-survival mode and high-throughput buffered mode. (src/utils/debug.ts)

**bundled mode** -- The runtime state where cc executes as a Bun-compiled standalone executable, detected via Bun.embeddedFiles, which changes settings loading, update handling, and feature availability. Bundled mode enables single-binary distribution without requiring users to install Bun or Node.js. (src/utils/bundledMode.ts)

**bypass-immune** -- A property of safety checks that prevents them from being overridden by bypassPermissions mode, auto mode classifiers, or PreToolUse hook approvals, ensuring that critical security boundaries cannot be circumvented. Bypass-immunity is the highest enforcement level in the permission hierarchy. (src/utils/permissions/)

**capacity wake signal** -- A signaling mechanism that wakes the work-polling loop immediately when a session completes, reducing latency between session completion and new work assignment from the polling interval to near-zero. Capacity wake signals implement an event-driven alternative to polling for multi-agent task allocation. (src/bridge/)

**character interning** -- The optimization pattern in terminal rendering where unique characters are stored once in a pool and referenced by integer ID, enabling diff operations to compare IDs instead of strings. Character interning reduces memory allocation and comparison cost in the terminal renderer. (src/ink/screen.ts)

**checkmark transition** -- A UI state in the permission dialog where an auto-approved action briefly shows a dimmed checkmark indicator before being removed, with different display durations based on terminal focus. Checkmark transitions provide visual feedback for auto-approved operations without interrupting the user. (src/hooks/useCanUseTool.tsx)

**checkpoint-restore** -- The ability to save and resume agent state across sessions, implemented via JSONL session persistence and tombstone entries. Checkpoint-restore enables long-running tasks to span multiple sessions, with the harness reconstructing context from persisted state. (src/utils/sessionStorage.ts)

**combined abort signal** -- The pattern of merging a parent abort signal with a timeout signal via createCombinedAbortSignal, used across all hook execution paths to enforce timeout boundaries. Combined signals provide unified abort semantics from two independent sources: user cancellation and timeout expiration. (src/utils/hooks/)

**command registry** -- The memoized collection of all Command objects assembled from built-in, skill-directory, plugin, bundled, and MCP sources, filtered by availability and feature gates. The command registry is the central index of all user-invocable slash commands. (src/commands.ts)

**command shadowing** -- The precedence mechanism where commands loaded earlier in the array take priority over later commands with the same name during findCommand lookup. Shadowing enables user-defined commands to override built-in commands of the same name. (src/commands.ts)

**compact boundary** -- A SystemCompactBoundaryMessage that records the compaction type, pre-compact token count, and the UUID of the last message before compaction. Compact boundaries enable the harness to reconstruct the conversation timeline after compaction by marking where summarization occurred. (src/services/compact/)

**compaction** -- The process of reducing conversation context length by summarizing or discarding older messages to stay within token budgets. Compaction is the core context management mechanism in cc, implementing HER Pattern 5 (Progressive Context Compaction) across multiple severity levels. (src/services/compact/)

**completion checker** -- A type-specific function registered via registerCompletionChecker() that determines remote task completion by checking external state (e.g., PR merge status) on every poll tick. Completion checkers decouple the harness from the specifics of how different remote task types signal completion. (src/tasks/RemoteAgentTask/)

**continue-vs-spawn decision** -- The coordinator's decision whether to continue an existing worker via SendMessage or spawn a fresh worker via AgentTool, based on context overlap between the worker's current context and the next task. This decision optimizes resource usage by reusing workers when their context is relevant. (src/coordinator/coordinatorMode.ts)

**cron jitter** -- Deterministic per-task delay applied to cron fire times to prevent thundering-herd conditions, derived from the task ID for reproducibility across restarts. Cron jitter ensures that multiple scheduled tasks do not fire simultaneously, distributing load evenly over time. (src/utils/cron.ts)

**Cross-App Access** -- An enterprise authentication pattern where MCP servers authenticate through a corporate Identity Provider (IdP) rather than their own OAuth flow, enabling SSO across all MCP servers. Cross-App Access simplifies authentication for enterprise deployments by centralizing identity management. (src/services/mcp/auth.ts)

**damage tracking** -- Maintaining a bounding rectangle of cells that changed during a render to limit the diff algorithm to only the modified region, skipping unchanged cells. Damage tracking is a rendering optimization that reduces the computational cost of screen differencing. (src/ink/screen.ts)

**debug log level** -- A five-tier verbosity filter (verbose, debug, info, warn, error) controlling which messages are written to the debug log file, configured via CLAUDE_CODE_DEBUG_LOG_LEVEL. Debug log levels enable selective output filtering without modifying code. (src/utils/debug.ts)

**deferred tool** -- A tool not included in the initial prompt but loaded on demand via ToolSearch when the model requests it, reducing prompt size. Deferred tools implement progressive tool expansion, trading a small first-use latency cost for significant prompt-size savings on every query. (src/tools/ToolSearchTool/)

**diminishing-returns detection** -- The heuristic in checkTokenBudget() that compares token deltas across consecutive loop iterations, stopping the query loop when progress falls below DIMINISHING_THRESHOLD for at least 3 continuations. This detection prevents the agent from continuing to consume resources when it is no longer making meaningful progress. (src/query/tokenBudget.ts)

**directory contract** -- The consistent subdirectory pattern each tool follows: a main implementation file, a prompt.ts for model description, and optionally a UI.tsx for terminal rendering. The directory contract provides a predictable structure that makes new tools discoverable by convention. (src/tools/)

**disk-cached feature flag** -- A GrowthBook feature flag value persisted to ~/.claude.json by a previous process, read without network I/O as a fallback when the in-memory payload is unavailable. Disk-cached flags enable feature gating to work even when the GrowthBook server is unreachable. (src/services/analytics/growthbook.ts)

**DNS-rebinding protection** -- The pattern of validating DNS-resolved IP addresses and passing the validated address directly to the TCP socket, closing the window where an attacker could serve different IPs on successive DNS resolutions. DNS-rebinding protection prevents time-of-check-to-time-of-use attacks on web tool connections. (src/utils/hooks/ssrfGuard.ts)

**domain-based permission rule** -- A permission rule that matches web tool access at the hostname level using the domain:hostname pattern, enabling fine-grained access control without requiring full URL specification. Domain-based rules provide a middle ground between per-URL and blanket permission policies. (src/utils/permissions/)

**dynamic-pacing loop** -- A /loop mode using ScheduleWakeup with simple relative delays instead of cron expressions, allowing the loop to adjust its pace based on task progress and prompt cache warmth. Dynamic pacing optimizes for cache efficiency by avoiding wake-ups that would miss the 5-minute cache window. (src/utils/cron.ts)

**env var interpolation** -- The mechanism in HTTP hooks that replaces $VAR_NAME or ${VAR_NAME} patterns in header values with environment variable values, gated by an allowedEnvVars allowlist. Env var interpolation enables dynamic header construction while preventing unauthorized access to sensitive environment variables. (src/schemas/hooks.ts)

**ephemeral worktree pattern** -- An exact-shape regex pattern that matches throwaway worktrees created by automated processes (AgentTool, WorkflowTool, bridgeMain) without matching user-named worktrees, used by cleanupStaleAgentWorktrees to avoid accidental deletion. The ephemeral pattern distinguishes agent-created worktrees from user worktrees. (src/utils/worktree.ts)

**fast-path routing** -- The pattern in cli.tsx where the harness decides whether to load the model at all, routing to specialized handlers for version checks, bridge mode, daemon mode, and other non-model operations before falling through to the full CLI. Fast-path routing reduces startup latency for common non-interactive operations. (src/entrypoints/cli.tsx)

**first-source-wins** -- The policy within the policySettings source where the highest-priority policy source with content takes over entirely, rather than merging with lower-priority policy sources. First-source-wins prevents conflicting enterprise policies from being merged in unintended ways. (src/utils/settings/)

**generator-evaluator** -- A multi-agent pattern where one agent produces output and another evaluates it, used in cc's coordinator for quality gates. The generator-evaluator pattern implements separation of concerns between creation and validation, addressing the self-evaluation bias identified in HER. (HER §9.2)

**ghost text** -- An inline completion hint rendered as dimmed text after the cursor position, providing the most likely suggestion without requiring the user to open a suggestion menu. Ghost text reduces typing effort by predicting and displaying completions inline. (src/hooks/useTypeahead.tsx)

**hold-to-talk** -- A voice input protocol where the user presses and holds a key to record audio, releasing it to send for transcription, with auto-repeat key event handling and release timeout detection. Hold-to-talk provides a push-to-talk interface for voice input in terminal environments. (src/hooks/useVoice.ts)

**hook event broadcasting** -- The lightweight pub/sub system in hookEvents.ts that broadcasts hook execution progress to SDK consumers and the UI, with a pending-events buffer for late-registered handlers. Event broadcasting enables external systems to observe the hook lifecycle in real time. (src/utils/hooks/hookEvents.ts)

**in-process vs process-based shutdown** -- The distinction between aborting an in-process teammate's AbortController (shared process) versus calling gracefulShutdown() for a process-based teammate (separate process), necessary because in-process teammates cannot be terminated by killing the process. This distinction ensures correct cleanup regardless of the teammate's execution model. (src/tools/AgentTool/)

**interview phase** -- An optional plan-mode phase (gated by tengu_plan_mode_interview_phase or USER_TYPE=ant) where the agent asks clarifying questions before finalizing the plan. The interview phase implements the HER principle that humans should steer before agents execute, reducing specification gaming. (src/utils/planModeV2.ts)

**IPv4-mapped IPv6 bypass** -- An SSRF attack vector where a private IPv4 address is encoded as an IPv6 address using the ::ffff:X.X.X.X format, bypassing IPv4-only address checks. cc's SSRF guard explicitly checks for IPv4-mapped IPv6 addresses to prevent this bypass. (src/utils/hooks/ssrfGuard.ts)

**keybinding context** -- A string identifier that determines when a keyboard handler is active, enabling context-aware key routing where the most specific context takes priority. Keybinding contexts prevent key conflicts by ensuring that only the handler for the current UI state receives keypress events. (src/hooks/useGlobalKeybindings.tsx)

**latest symlink** -- An advisory symlink at ~/.claude/debug/latest pointing to the current session debug log file, created once per session by updateLatestDebugLogSymlink. The latest symlink provides a stable path for accessing the most recent debug log without knowing the session ID. (src/utils/debug.ts)

**metadata re-append** -- The pattern of unconditionally re-writing session metadata entries near EOF after compaction or on exit to keep them within the 64KB tail window for fast reads. Metadata re-append ensures that session metadata is always accessible via the head/tail read optimization. (src/utils/sessionStorage.ts)

**missed-task detection** -- The startup check that identifies cron tasks whose scheduled fire time passed while the REPL was closed, surfacing them for catch-up. Missed-task detection ensures that scheduled work is not silently dropped when the agent is not running. (src/utils/cron.ts)

**mode trap** -- A state the model can enter but cannot leave, such as plan mode in channel-based sessions where the approval dialog cannot be displayed. Mode traps are a class of UI deadlock where the agent's operational mode prevents the user interface required to exit that mode. (src/utils/planModeV2.ts)

**MCP requirement filtering** -- The mechanism that removes agents from the active list when their declared requiredMcpServers do not match any configured MCP servers, preventing runtime failures from missing infrastructure. Requirement filtering is a startup-time validation that prevents agents from being dispatched when their dependencies are unavailable. (src/tools/AgentTool/)

**MCP server connection** -- A discriminated union tracking the lifecycle state of an MCP server connection through five states: pending, connected, failed, needs-auth, and disabled. The connection state machine enables the harness to reason about server availability and route tool calls appropriately. (src/services/mcp/)

**MCP tool collapse classification** -- The heuristic categorization of MCP tools for compaction, determining which tool results can be safely summarized during context compaction based on tool name patterns, result size, and recency. Collapse classification prevents loss of critical MCP tool results during compaction. (src/services/mcp/)

**MCP transport** -- The communication channel between cc and an MCP server, implemented as stdio (child process), SSE, HTTP, WebSocket, or SDK (in-process). MCP transports abstract over the communication protocol, enabling the same tool interface regardless of how the server is connected. (src/services/mcp/)

**onChange diff** -- The centralized diff pattern in onChangeAppState that compares old and new state to trigger side effects, ensuring any setState call automatically propagates changes to all consumers. The onChange diff implements reactive state propagation without requiring explicit subscription management. (src/state/onChangeAppState.ts)

**one-shot cron task** -- A cron task that fires once and is auto-deleted, as opposed to a recurring task which reschedules after firing. One-shot tasks implement the "remind me at X" pattern, providing time-based triggers without persistent scheduling overhead. (src/utils/cron.ts)

**passthrough permission** -- A permission result type (behavior: 'passthrough') where the tool invocation requires user approval unless a rule explicitly allows it, used for MCP tools whose code is not controlled by the harness. Passthrough permissions treat third-party tools with caution since the harness cannot vouch for their behavior. (src/services/mcp/)

**permission context** -- A frozen helper object that encapsulates the resolve function, abort detection, logging, and queue operations for a single permission evaluation, threaded through all handlers. The permission context provides a clean interface for the complex state involved in permission decisions. (src/hooks/useCanUseTool.tsx)

**PewterLedgerVariant** -- A GrowthBook experiment variant controlling plan file size in plan mode V2, with arms trim/cut/cap representing progressively stricter verbosity reduction. PewterLedgerVariant enables A/B testing of plan verbosity to find the optimal balance between detail and context efficiency. (src/utils/planModeV2.ts)

**PII marker type** -- A TypeScript type set to never that forces explicit casting before including string values in analytics metadata, preventing accidental inclusion of code snippets or file paths in events. The never-typed marker implements type-level enforcement of privacy constraints. (src/services/analytics/)

**polling-based event stream** -- A pull-based coordination model where the local session periodically fetches remote agent events via HTTP, as opposed to push-based WebSocket or webhook callbacks. Polling provides a simpler and more firewall-friendly coordination mechanism than push-based alternatives. (src/tasks/RemoteAgentTask/)

**prePlanMode** -- A field in the permission context that stores the user's permission mode before entering plan mode, used to restore it on exit. PrePlanMode ensures that the transition out of plan mode returns the agent to its previous operational mode. (src/utils/planModeV2.ts)

**queue-then-drain** -- The pattern where events are buffered in an in-memory array until a sink is attached, then drained asynchronously via queueMicrotask to avoid blocking startup. Queue-then-drain enables analytics to work correctly even when the analytics backend is not yet initialized. (src/services/analytics/)

**remote trigger** -- A REST API configuration object on claude.ai CCR that defines a cloud-side agent; invoked via POST /v1/code/triggers/{id}/run to start a remote session. Remote triggers enable the harness to dispatch long-running tasks to the cloud, freeing the local session for other work. (src/tools/RemoteTriggerTool/)

**scratchpad** -- A feature-gated shared directory for durable cross-worker knowledge sharing in coordinator mode, allowing workers to read and write without permission prompts. Scratchpads implement the HER principle of file-based inter-agent communication, providing a shared workspace that persists across tool rounds. (src/coordinator/)

**server signature deduplication** -- The signature-based approach to preventing duplicate MCP server configurations, where stdio servers use the command array and URL-based servers use the unwrapped CCR proxy URL as the deduplication key. Signature deduplication prevents redundant server connections from conflicting configuration sources. (src/services/mcp/)

**session-only cron** -- A cron task held in process memory that never touches the filesystem and dies with the process when durable is false. Session-only crons are appropriate for temporary scheduling needs that should not survive process restarts. (src/utils/cron.ts)

**session-retry loop** -- A retry pattern in MCP tool calls that automatically retries once on McpSessionExpiredError after clearing the connection cache and obtaining a fresh client. Session-retry handles the common case where MCP server sessions expire between tool calls. (src/services/mcp/)

**session token unlink** -- The security pattern of deleting a secret file from disk after the infrastructure that depends on it has been confirmed running, ensuring the secret is available for retry during init but invisible to the agent loop during operation. Token unlinking prevents the agent from accessing authentication secrets that should only be used during initialization. (src/bridge/)

**structured output enforcement** -- The pattern where a PostToolUse function hook checks whether the agent has called StructuredOutputTool and, if not, blocks the response and prompts the agent to use it. Structured output enforcement guarantees that agent responses conform to expected schemas when required by the calling context. (src/schemas/hooks.ts)

**style transition caching** -- Pre-computing and caching the ANSI escape sequence needed to transition between any two style IDs, enabling zero-allocation style changes after the first call. Style transition caching eliminates repeated string construction in the terminal rendering hot path. (src/ink/screen.ts)

**synthesis** -- The coordinator's process of reading and understanding worker findings before delegating follow-up work, producing a specific implementation spec with file paths, line numbers, and expected outcomes. Synthesis is the coordinator's core function: transforming raw research into actionable implementation directives. (src/coordinator/coordinatorMode.ts)

**task assignment notification** -- A JSON message sent via the teammate mailbox when a task's owner changes, ensuring the newly assigned teammate learns about the work without polling the task list. Assignment notifications implement push-based task routing, reducing latency compared to polling-based alternatives. (src/utils/tasks.ts)

**worker** -- A subagent with subagent_type: worker dispatched by the coordinator to research, implement, or verify code changes in an isolated context. Workers are the execution units in the coordinator pattern, operating under the coordinator's direction without direct user interaction. (src/coordinator/coordinatorMode.ts)

**worktree path notice** -- An advisory message generated by buildWorktreeNotice() that warns fork children running in isolated worktrees to translate inherited context paths from the parent's working directory to the worktree root. Path notices prevent path confusion when subagents operate in worktrees with different working directories. (src/tools/AgentTool/)

**append-only discipline** -- The design principle that no entry is ever modified after write; deletions are represented by omission or explicit removal. Core to cc's session persistence model, append-only discipline guarantees that session logs form an immutable audit trail recoverable after crashes or context loss. (src/utils/sessionStorage.ts)

**auto-background** -- A mechanism that automatically transitions a synchronous subagent to asynchronous execution after a configurable timeout (default 120 seconds). This prevents long-running subagents from blocking the parent agent's query loop while still delivering results when ready. (src/tools/AgentTool/AgentTool.tsx)

**autocompact** -- An automatic compaction trigger that fires when the context window approaches capacity, initiated by the query loop. Autocompact represents the proactive stage of context management, distinguishing it from reactive compaction triggered by API rejection. (src/services/compact/autoCompact.ts)

**back-pressure** -- A mechanism that throttles or redirects agent activity when resource limits (tokens, cost, time) are approached. Back-pressure implements the HER principle that quality gates should feed failures back into agent context while keeping successes silent. (HER §20)

**back-pressure stack** -- The tiered quality gate pipeline (Type System -> Linter -> Unit Tests -> Integration Tests -> E2E Tests) where only failures surface to agent context, making success silent. This stack embodies the principle that deterministic checks are more reliable than LLM self-evaluation. (HER §20)

**bridge** -- A bidirectional transport layer connecting cc's in-process agent loop to external orchestrators (IDE extensions, web clients, SDK daemons), implementing message translation, session lifecycle management, OAuth token refresh, and permission proxying. The bridge enables cc to function as a headless backend while retaining full interactive capability. (src/bridge/)

**bubble permission model** -- A permission model where fork child processes route permission requests to the parent's terminal via IPC, since the child process has no terminal of its own, controlled by permissionMode: 'bubble' on the agent definition. This ensures that even isolated subagents remain subject to user oversight. (src/tools/AgentTool/)

**cache boundary** -- A control token (SYSTEM_PROMPT_DYNAMIC_BOUNDARY) that splits the system prompt array into static (globally cacheable) and dynamic (per-session) segments for API-level prompt caching. The boundary enables the Anthropic API to cache the static prefix across requests while invalidating only the dynamic suffix. (src/constants/prompts.ts)

**cache break** -- The event where the API's prompt cache is invalidated, detected by comparing cache_creation_input_tokens against the previous request's, requiring full re-processing of input tokens. Cache breaks represent a significant cost event because the entire cached prefix must be re-processed. (src/services/api/claude.ts)

**cache editing** -- The Anthropic API mechanism for removing tool results from the cached prompt prefix without invalidating the cache, used by the cached microcompact path. Cache editing enables targeted context reduction while preserving the cache hit on preceding messages. (src/services/compact/)

**cache-safe params** -- A set of API request parameters (system prompt, tools, model, context messages) captured immediately after assembly to ensure byte-identical prefixes between parent and fork children for prompt cache hits. This optimization allows multiple fork children to share the same cached prompt prefix, reducing cost. (src/tools/AgentTool/)

**CACHED_OR_BLOCKING** -- A GrowthBook gate evaluation pattern that returns cached true values immediately but blocks on fresh server values when the cache says false, preventing unfair blocking from stale negative values. This asymmetric strategy optimizes for the common case where features are enabled. (src/services/analytics/growthbook.ts)

**circuit breaker** -- The mechanism that stops retrying autocompact after MAX_CONSECUTIVE_AUTOCOMPACT_FAILURES consecutive failures, preventing unbounded API waste. The circuit breaker implements a fail-closed strategy: once triggered, it requires user intervention rather than continuing to consume resources. (src/services/compact/autoCompact.ts)

**circuit-broken auto mode** -- A state in which auto mode has been permanently disabled for the current session after a server-side kill switch fires, preventing re-entry even on subsequent GrowthBook refreshes. This is a safety mechanism that overrides local configuration when the platform detects unsafe behavior. (src/utils/permissions/autoModeState.ts)

**classifier** -- A function that maps a shell command string to a risk band (read, write, destructive) to drive permission decisions. Classifiers enable the harness to make fast, deterministic safety judgments without invoking the model, reducing latency for common operations. (src/utils/permissions/bashClassifier.ts)

**clip stack** -- A stack of intersecting clip regions maintained during rendering to implement overflow: hidden behavior, where nested clips are intersected and writes are clipped to the innermost region. The clip stack enables efficient spatial culling of terminal output. (src/ink/)

**coordinator mode** -- An operational mode where the parent agent orchestrates worker subagents instead of implementing directly, gated by a compile-time feature gate and a runtime environment variable. Coordinator mode implements the HER Generator-Evaluator pattern at the process level. (src/coordinator/coordinatorMode.ts)

**compaction death spiral** -- The self-reinforcing failure mode where each failed compaction attempt adds tokens to the context, making the next attempt less likely to succeed. The death spiral is a primary motivation for cc's circuit breaker and hard reset fallback. (HER §6.1)

**compaction hierarchy** -- The five-stage context rescue pipeline: history_snip -> Microcompact -> Context Collapse -> Autocompact -> Hard Reset. Each stage applies progressively more aggressive summarization, with the final stage requiring user intervention. (src/services/compact/)

**companion bones** -- The deterministic-from-identity traits of a Buddy companion (species, eye, hat, shiny, stats), always re-derived from a seeded PRNG and never persisted, preventing forgery or gaming. Bones ensure that the same user always sees the same companion regardless of session state. (src/buddy/companion.ts)

**companion soul** -- The persisted companion data stored in the user global config, merged with freshly-derived bones at display time so that stale bones fields in old-format configs get overridden. Soul data represents the mutable state of the companion (experience, achievements) while bones remain fixed. (src/buddy/companion.ts)

**compound failure problem** -- The mathematical result that per-step reliability compounds poorly over many steps: 95% per-step yields only 36% over 20 steps, and doubling task duration quadruples failure rate. This result from HER §6 motivates the emphasis on back-pressure and quality gates in long-running agents. (HER §6)

**conditional rule scoping** -- The mechanism by which .claude/rules/*.md files with frontmatter paths: globs are filtered at query time to inject only instructions relevant to the current file being operated on. Conditional scoping prevents prompt bloat from loading all project rules unconditionally. (src/utils/claudemd.ts)

**conditional skill** -- A skill with a paths frontmatter field that is not loaded into the tool listing at startup but is activated when the model operates on files matching the declared glob patterns. Conditional skills reduce initial prompt size while remaining discoverable when contextually relevant. (src/tools/SkillTool/)

**consolidation lock** -- A filesystem-based lock file whose mtime serves as the lastConsolidatedAt timestamp and whose body contains the holder's PID, preventing concurrent consolidation across processes. The lock implements the HER principle of using filesystem state for cross-process coordination. (src/services/autoDream/consolidationLock.ts)

**content-hash temp path** -- A temporary file path derived from a content hash rather than a random UUID, used to preserve API prompt cache hits across process boundaries. Content-hash paths ensure that identical content always maps to the same file, enabling cache reuse. (src/utils/)

**context collapse** -- A severe compaction stage triggered when token usage exceeds critical thresholds, aggressively summarizing the full conversation. Context collapse is more destructive than microcompact or autocompact because it discards most of the conversation history. (src/services/compact/)

**cost gate** -- A mechanism that aborts or degrades a session when spend exceeds a configured threshold, implementing three tiers: soft cap (warning + model downgrade), hard cap (stop new tasks), and kill switch (immediate abort). Cost gates are the financial counterpart to the back-pressure stack's quality gates. (HER §13)

**cost metering** -- The real-time tracking of token and dollar costs per session and per model, implemented in src/cost-tracker.ts via addToTotalSessionCost and related functions. Cost metering provides the observability foundation for cost gates and spend-rate monitoring. (src/cost-tracker.ts)

**cross-reference table** -- A structured mapping from HER patterns, failure modes, or best practices to the specific chapters and source files that address them. The cross-reference table enables readers to navigate from theoretical framework to concrete implementation. (Chapter 2)

**debug filter pattern** -- A module or function name pattern specified via --debug=pattern that filters debug output to matching entries, parsed by parseDebugFilter and applied by shouldShowDebugMessage. Filter patterns reduce debug log noise to relevant subsystems. (src/utils/debug.ts)

**defense-in-depth** -- A security architecture pattern where multiple independent defensive layers are stacked, so that failure of one layer does not compromise the system. cc implements this for prompt injection with permission models, classifiers, SSRF guards, and denial tracking. (HER §12)

**denial tracking** -- A subsystem that tracks consecutive and total permission denials to prevent classifier death spirals, falling back to interactive prompting after configurable thresholds. Denial tracking prevents the agent from repeatedly attempting blocked operations. (src/utils/permissions/denialTracking.ts)

**denial workaround guidance** -- The DENIAL_WORKAROUND_GUIDANCE pattern that constrains model behavior after permission denials, allowing reasonable workarounds while preventing malicious bypasses. This guidance prevents the agent from escalating to unsafe alternatives when denied. (src/constants/prompts.ts)

**deny-first evaluation** -- The permission pipeline design where deny rules are checked before allow rules, ensuring that a deny rule from any source always takes precedence over an allow rule. Deny-first evaluation is a security-critical ordering that prevents accidental privilege escalation. (src/utils/permissions/)

**diminishing-returns detector** -- The heuristic in checkTokenBudget() that compares token deltas across consecutive loop iterations, stopping the query loop when progress falls below DIMINISHING_THRESHOLD for at least 3 continuations. This detector prevents the agent from continuing to consume resources when it is no longer making meaningful progress. (src/query/tokenBudget.ts)

**dispatch pipeline** -- The ordered sequence of stages a tool invocation passes through (validation, permission, execution, post-hooks), from model intent to result. The dispatch pipeline is the central safety mechanism of cc, implementing defense-in-depth at the tool level. (src/services/tools/toolExecution.ts)

**dream consolidation** -- A background process that reviews, deduplicates, and prunes memory files during idle time, implemented via a three-gate system (time, sessions, lock) and a forked subagent. Dream consolidation implements HER Pattern 4, moving knowledge from ephemeral session context to durable memory files. (src/services/autoDream/)

**drop-in convention** -- The managed-settings.d directory pattern where *.json files are loaded in alphabetical order and merged on top of the base managed-settings.json, matching the systemd/sudoers drop-in convention. Drop-in conventions enable enterprise administrators to layer configuration without modifying base files. (src/utils/settings/)

**dynamic skill discovery** -- The process of discovering skills from .claude/skills directories encountered during file operations, as opposed to the startup-time scan. Dynamic discovery ensures that skills relevant to newly-accessed directories become available without restarting the session. (src/skills/)

**eager settings loading** -- The pattern of parsing CLI flags before the Commander program is constructed, necessary when flags affect initialization behavior. Eager loading enables flags like --debug or --model to influence startup before the full CLI framework is initialized. (src/entrypoints/cli.tsx)

**early input buffer** -- The startCapturingEarlyInput() mechanism that buffers terminal keystrokes while the main module loads, flushing them into the REPL once the Ink rendering pipeline is ready. This prevents keystroke loss during the startup period when the terminal is not yet accepting input. (src/entrypoints/)

**entitlement-activation gate** -- The two-layer availability pattern where a tool requires both entitlement (enrolled in experiment/feature flag) and activation (explicit user opt-in), as implemented by Brief's isBriefEntitled() + isBriefEnabled(). This gate ensures that experimental features are not accidentally exposed to users who have not opted in. (src/tools/BriefTool/)

**entrypoint** -- The MEMORY.md index file that is always loaded into context, serving as the compact catalog of available memory topic files, capped at 200 lines and 25KB. The entrypoint implements the HER principle of tiered memory: a compact always-loaded index plus on-demand detail files. (src/memdir/)

**escalation record** -- A context-rich payload attached to human-in-the-loop prompts carrying the agent's confidence, intent, prior attempts, options, and recommended action. Escalation records enable informed human decision-making at critical junctures. (HER §11)

**evaluator ouroboros** -- The degenerate case where using an LLM evaluator to grade LLM output creates a circular validation loop, named after the Ouroboros problem identified in HER §9.2. The ouroboros problem motivates the preference for deterministic evaluation (tests, linters) over LLM evaluation. (HER §9.2)

**exit code semantics** -- The three patterns (block-on-2, show-on-2, output-as-input) that define how a hook's exit code is interpreted for each lifecycle event. Exit code semantics provide a compact protocol for hook-to-harness communication without requiring structured output formats. (src/schemas/hooks.ts)

**extraction agent** -- A forked subagent with write access limited to a single file (the session memory file), implementing HER Pattern 7 for isolated context extraction. The extraction agent's restricted write scope prevents it from modifying the working tree while preserving the ability to persist observations. (src/services/SessionMemory/)

**fail-closed** -- A safety design principle where uncertainty or error states default to the most restrictive behavior, preventing data loss from false negatives. Contrasted with fail-open, where errors default to permissive behavior. cc's permission system, classifiers, and SSRF guards all default to fail-closed. (src/utils/permissions/)

**fail-closed default** -- A default value for a safety-critical method that assumes the most restrictive behavior when the method is not explicitly implemented, ensuring that omission cannot lead to unsafe execution. Fail-closed defaults are a structural control that makes unsafe configurations impossible by default. (src/utils/permissions/)

**failure mode** -- A categorization of how long-running agents can fail, as defined by HER Section 6, covering 17 specific modes from context rot to specification gaming. Failure modes provide a taxonomy for analyzing agent reliability and designing targeted mitigations. (HER §6)

**fast-resume path** -- An optimization that checks whether a worktree with the same slug already exists by reading the git internal pointer file directly, avoiding subprocess spawn overhead (~15ms) on every resume. The fast-resume path significantly reduces latency for the common case of resuming an existing worktree. (src/utils/worktree.ts)

**feature gate** -- A compile-time boundary checked via feature() from bun:bundle that determines which code exists in the external build. Feature gates enable cc to maintain a single codebase while producing different binaries for internal and external distribution. (src/entrypoints/)

**file-history backup** -- A content-hash-keyed backup created before file edits, stored at ~/.claude/file-history/{sessionId}/{sha256-hash}@v{N}, providing checkpoint-restore semantics for undo operations. File-history backups implement the HER principle of idempotent operations with rollback capability. (src/utils/fileHistory.ts)

**five-layer defense-in-depth** -- HER Section 12.1's security model consisting of prompt-level guardrails, schema-level restrictions, runtime approval, tool-level validation, and lifecycle hooks, each acting as a progressive filter on tool invocations. cc implements all five layers, making it the most complete public example of this architecture. (HER §12.1)

**fixed-point command stripping** -- The iterative approach to stripping env vars and wrapper commands from shell invocations, applying both operations until no new candidates are produced. Fixed-point stripping handles nested wrapper commands (e.g., `env FOO=bar npx prettier`) that a single pass would miss. (src/tools/BashTool/commandSemantics.ts)

**flag-level allowlisting** -- The practice of validating individual CLI flags for complex commands rather than using command-level regexes, implemented via COMMAND_ALLOWLIST with typed FlagArgType entries. Flag-level allowlisting prevents parser differentials where the allowlist and the shell disagree on command parsing. (src/tools/BashTool/bashSecurity.ts)

**fork** -- A child process created via Bun.fork() to run a subagent with its own memory and event loop, communicating via IPC. Forks provide process-level isolation for subagents, preventing a misbehaving subagent from corrupting the parent's state. (src/tools/AgentTool/forkSubagent.ts)

**fork boilerplate** -- An XML-tagged instruction block wrapped in FORK_BOILERPLATE_TAG that is injected into fork child messages to prevent recursive forking, enforce structured output format, and constrain the child's behavior to its directive scope. Fork boilerplate is a structural control that limits the blast radius of subagent misbehavior. (src/tools/AgentTool/)

**fork recursion guard** -- A dual-guard mechanism preventing infinite recursive agent spawning: a message-scan guard checking FORK_BOILERPLATE_TAG and a querySource guard surviving autocompact. The recursion guard ensures that subagents cannot spawn their own subagents without explicit authorization. (src/tools/AgentTool/)

**frontmatter** -- A YAML header delimited by --- markers at the top of a Markdown file, parsed into typed fields that configure agents, skills, or slash commands. Frontmatter enables declarative configuration of complex agent behavior without requiring code changes. (src/utils/frontmatterParser.ts)

**full jitter** -- A retry backoff strategy where the delay is uniformly distributed between 0 and the current backoff interval, preventing thundering herd effects when multiple agents retry simultaneously. Full jitter is superior to exponential backoff with fixed jitter for distributed systems. (src/services/api/)

**greenfield bias** -- The tendency for agent benchmarks and results to be more impressive on new projects without existing architecture constraints, versus brownfield work on existing codebases. Greenfield bias inflates perceived agent capability and should be accounted for when evaluating harness performance. (HER §18)

**grace period** -- A configurable delay (200ms in the interactive handler) that prevents accidental user keypresses from canceling an in-progress automated check. Grace periods reduce false-negative permission denials from keyboard bounce or accidental key presses. (src/hooks/useCanUseTool.tsx)

**guide** -- A configuration-driven constraint that shapes agent behavior, such as CLAUDE.md instructions, settings, or managed policies. Guides correspond to Martin Fowler's taxonomy of "guides" -- advisory controls that influence behavior without enforcing it. (HER §4)

**hard reset** -- The fallback when all compaction stages have failed, requiring user intervention to resolve prompt_too_long errors. Hard reset is the fail-closed terminal state of the compaction hierarchy, ensuring that the agent never silently continues with corrupted context. (src/services/compact/)

**head/tail read** -- The optimization pattern of reading only the first and last 64KB of a JSONL file to extract metadata (first prompt, last title, last tag) without parsing the full content. Head/tail reads enable fast session listing without loading entire session histories. (src/utils/sessionStorage.ts)

**head-limit pagination** -- The pattern in GrepTool and GlobTool where a default limit (250) caps result size, with explicit pagination signaling when truncation occurs, enabling the model to paginate with offset. Head-limit pagination prevents context window overflow from large search results. (src/tools/GrepTool/, src/tools/GlobTool/)

**high water mark** -- A persistent counter file (.highwatermark) that stores the maximum task ID ever assigned, preventing ID reuse after deletion or reset operations. The high water mark ensures that task IDs are monotonically increasing across process restarts. (src/utils/tasks.ts)

**hook** -- A deterministic lifecycle event handler (command, prompt, HTTP, agent, or function type) triggered at PreToolUse, PostToolUse, or other defined points. Hooks implement HER Pattern 12, enabling external systems to observe and control the agent's tool dispatch pipeline. (src/schemas/hooks.ts)

**hook matcher** -- A grouping construct that associates one or more hooks with a matcher string, where the matcher is event-specific (tool names for PreToolUse/PostToolUse, notification types for Notification, etc.). Hook matchers enable fine-grained targeting of hook execution. (src/utils/hooks/hooksSettings.ts)

**harness** -- The outer runtime layer that orchestrates the model's interactions with tools, permissions, hooks, memory, and the user. The harness is the complete infrastructure that wraps the model, implementing the equation Agent = Model + Harness. (HER §1)

**immediate mode** -- The debug writer mode where writes use appendFileSync to guarantee crash survival, active when --debug or DEBUG env var is set. Immediate mode trades write performance for crash safety, ensuring that debug logs are never lost due to process termination. (src/utils/debug.ts)

**internal path carve-out** -- An exception in the filesystem permission layer that allows the agent to access specific internal paths (plan files, scratchpads, session memory) without explicit permission, even when those paths reside within otherwise dangerous directories. Carve-outs prevent the permission system from blocking its own infrastructure. (src/utils/permissions/filesystem.ts)

**iron gate** -- The GrowthBook feature flag (tengu_iron_gate_closed) that controls whether the auto mode classifier fails closed (deny) or fails open (prompt) when the classifier is unavailable. Iron gate implements the fail-closed principle at the feature-flag level. (src/utils/permissions/)

**keyword scoring** -- The weighted search algorithm in ToolSearchTool that parses queries into terms and matches them against tool name parts, search hints, and descriptions with different weights (10/12 for exact name match, 5/6 for partial, 4 for hint, 2 for description). Keyword scoring balances precision and recall in tool discovery. (src/tools/ToolSearchTool/)

**lazy require** -- A pattern using require() at call time instead of import time to break circular dependencies between modules. Lazy require defers module resolution until the function is actually called, breaking import-time cycles. (src/utils/)

**lazySchema** -- A wrapper function that defers Zod schema evaluation until first call, ensuring runtime feature flags are populated before schema construction. LazySchema solves the boot-order problem where schemas reference feature-gated values that are not yet initialized at import time. (src/Tool.ts)

**loop detector** -- A sliding-window monitor that flags identical tool calls within a configurable time window, preventing infinite retry loops. The loop detector is a structural control that prevents HER Failure Mode 6.7 (Infinite Loops) from consuming unbounded resources. (src/query/)

**mailbox delivery** -- A file-based message delivery mechanism for inter-teammate communication that persists across process restarts, used as a fallback when in-process queue delivery is not possible. Mailbox delivery implements the HER principle of file-based inter-agent communication. (src/tools/SendMessageTool/)

**managed settings** -- Enterprise-administered settings from the policySettings source, loaded from remote API, MDM (HKLM/plist), managed-settings.json (with drop-in directory), or HKCU registry. Managed settings enable organizations to enforce configuration policies across all team members. (src/utils/settings/)

**manifest** -- The book-build state file (manifest.json) tracking all 57 chapters, their source files, audit pass counts, and convergence status. The manifest implements the HER principle of structured state tracking for long-running multi-step processes. (src/utils/tasks.ts)

**memory** -- A persistent note stored in the memdir tiered-memory system, typed as user, feedback, project, or reference, with age tracking. Memory entries implement HER Pattern 3 (Tiered Memory), providing durable knowledge that persists across session boundaries. (src/memdir/)

**memory correction hint** -- A just-in-time behavioral nudge appended to rejection messages when auto-memory is enabled, steering the model toward saving feedback at the moment of denial. Memory correction hints convert negative experiences into durable learning. (src/constants/prompts.ts)

**memory loading hierarchy** -- The priority-ordered cascade of CLAUDE.md file loading: managed, user, project, local, auto-mem, team-mem, where later-loaded files take precedence. The loading hierarchy ensures that project-specific instructions override user defaults, and team instructions override project defaults. (src/utils/claudemd.ts)

**memdir** -- The disk-based tiered memory subsystem at ~/.claude/memories/ that stores, scans, and retrieves memory .md files for context injection. Memdir implements HER Pattern 3, providing persistent memory that survives context resets and session boundaries. (src/memdir/)

**message normalization** -- The process of splitting multi-block messages into individual single-block messages with derived UUIDs, ensuring API structural requirements are met. Normalization handles the mismatch between cc's flexible message model and the Anthropic API's structural constraints. (src/utils/messages.ts)

**meta-chapter** -- A structural chapter that maps between the HER theoretical framework and the technical chapters, serving as connective tissue rather than a source-code deep dive. Meta-chapters (2, 53, 54, 55, 56, 57) synthesize patterns across subsystems rather than analyzing individual files. (Chapter 2)

**microcompact** -- A lightweight compaction pass that trims tool results and history_snip blocks without invoking the model, preserving recent turns intact. Microcompact is the least destructive compaction stage, operating at the token level rather than the semantic level. (src/services/compact/microCompact.ts)

**observation masking** -- The technique of redacting or summarizing prior tool outputs to reduce token cost while preserving decision-relevant information. Observation masking implements HER §8.1, achieving up to 52% cost reduction by suppressing successful tool outputs. (HER §8.1)

**once-hook** -- A hook with once: true that is removed after its first execution, useful for one-time setup commands. Once-hooks enable initialization logic that should not fire on every tool invocation. (src/schemas/hooks.ts)

**Ouroboros problem** -- When both generator and evaluator are LLMs, who validates the validator? Named in HER §9.2 and §18.2 as a fundamental limitation of LLM-based evaluation. The Ouroboros problem motivates cc's preference for deterministic quality gates over LLM-based evaluation. (HER §9.2)

**packed cell layout** -- Storing terminal cell data as two consecutive Int32 elements (characterID and packed style/hyperlink/width) in a contiguous array, eliminating per-cell object allocation. Packed cell layout is a performance optimization that reduces GC pressure in the terminal renderer. (src/ink/screen.ts)

**parser differential** -- A security vulnerability where the validator and the shell interpreter disagree on the parsing of a command string, enabling bypass attacks (e.g., xargs -i vs -I). Parser differentials are a class of security bugs that arise from inconsistencies between validation and execution logic. (src/tools/BashTool/bashSecurity.ts)

**partial compaction** -- User-initiated compaction of a portion of the conversation, preserving either the prefix or suffix while summarizing the other half. Partial compaction gives users manual control over context management when automatic strategies are insufficient. (src/services/compact/)

**path safety validation** -- The process of checking file paths against dangerous file/directory lists, suspicious Windows patterns, and Claude configuration files before allowing auto-edit operations. Path safety validation prevents the agent from modifying its own configuration files or system-critical paths. (src/utils/permissions/filesystem.ts)

**pending state pattern** -- A module-scope variable with a feature() gate that is set during argv parsing and consumed later in the action handler, enabling dead-code elimination of feature-gated code paths. The pending state pattern bridges the gap between CLI flag parsing and the Commander action handler. (src/entrypoints/cli.tsx)

**permission mode** -- A named configuration governing tool-access behavior, such as default, plan, acceptEdits, bypassPermissions, dontAsk, or auto. Permission modes implement the HER principle of tiered access control, with each mode representing a different trust level. (src/utils/permissions/PermissionMode.ts)

**permission mode inheritance** -- The mechanism by which a teammate transitioning from plan mode to implementation inherits the team lead's permission mode, with the exception that plan mode is replaced by default mode to prevent accidental write restrictions. Mode inheritance ensures consistent safety behavior across team members. (src/tools/AgentTool/)

**placeholder tool_result** -- A synthetic tool_result block with identical placeholder text (FORK_PLACEHOLDER_RESULT) inserted for every tool_use block in the parent's assistant message, ensuring all fork children produce byte-identical API request prefixes up to the per-child directive. Placeholder results enable prompt cache sharing across fork children. (src/tools/AgentTool/)

**plan mode** -- A read-only operational mode that restricts the agent to exploration and planning before committing to file modifications. Plan mode implements HER Pattern 6 (Explore-Plan-Act Loop), separating the analysis phase from the implementation phase. (src/tools/EnterPlanModeTool/)

**preapproved URL** -- A URL whose hostname is on the PREAPPROVED_HOSTS allowlist, allowing WebFetchTool to skip explicit per-request permission checks for known-safe documentation sites. Preapproved URLs reduce permission fatigue for commonly-accessed reference documentation. (src/tools/WebFetchTool/preapproved.ts)

**PreToolUse** -- A hook event fired before a tool's call() method executes, allowing interception, modification, or denial of the invocation. PreToolUse hooks are the primary interception point for implementing organizational policies and safety checks. (src/schemas/hooks.ts)

**PostToolUse** -- A hook event fired after a tool's call() method completes, enabling logging, validation, or post-processing of results. PostToolUse hooks are the primary observation point for implementing audit trails and quality gates. (src/schemas/hooks.ts)

**progress bridge** -- The map that chains through consecutive legacy progress entries and rewrites subsequent messages' parentUuid to skip past them, maintaining chain integrity across entries no longer in the type union. Progress bridges enable backwards-compatible session log migration. (src/utils/sessionStorage.ts)

**progressive tool expansion** -- The mechanism by which cc starts with a small set of core tools and loads additional tools on demand via ToolSearch, reducing initial prompt size and improving selection accuracy. Progressive tool expansion implements HER Pattern 9, trading latency on first use for reduced prompt size on every query. (src/tools/ToolSearchTool/)

**prompt assembly** -- The process of composing the system prompt from multiple sections in a specific layer order, including static cacheable sections and dynamic per-session sections separated by a boundary marker. Prompt assembly implements the HER principle of scoped context construction, where each layer adds information without modifying previous layers. (src/constants/prompts.ts)

**quality-gates pipeline** -- A deterministic checkpoint pipeline wired into the query loop that evaluates tool results with lint/type-check/test gates and routes verdicts to pass/fail/escalate actions. Quality gates implement the HER back-pressure stack, where only failures surface to agent context. (HER §20)

**query loop** -- The async generator function in src/query.ts that orchestrates the cycle of API calls, tool dispatch, compaction, and budget checks; the central execution engine of cc. The query loop is the heartbeat of the agent, implementing HER Pattern 6 (Explore-Plan-Act Loop) at the operational level. (src/query.ts)

**queue-and-drain pattern** -- A message delivery pattern where incoming messages are queued in a pendingMessages array and drained at tool-round boundaries, preventing race conditions from mid-execution message injection. The queue-and-drain pattern ensures that message processing happens at safe points in the query loop. (src/tools/SendMessageTool/)

**read-before-write contract** -- The invariant enforced by Write, Edit, and NotebookEdit tools that the model must have read a file before modifying it, preventing blind overwrites and silent data loss. The read-before-write contract is a structural control that makes data loss from unverified writes impossible. (src/tools/FileWriteTool/, src/tools/FileEditTool/)

**readFileState** -- A Map<string, ReadFileEntry> maintained in ToolUseContext that records every file the model has read, along with content, modification timestamp, offset, and limit; used to enforce the read-before-write contract and read deduplication. readFileState is the bookkeeping mechanism behind the read-before-write contract. (src/services/tools/)

**reactive compact** -- An emergency compaction triggered reactively when the API rejects a request for exceeding the context window, using a more aggressive strategy than proactive autocompact. Reactive compaction is the agent's last automatic chance to recover from context overflow before requiring user intervention. (src/services/compact/)

**reading path** -- A recommended traversal order through the book's chapters, selected by reader role or goal (sequential, subsystem, or HER path). Reading paths enable non-linear navigation tailored to different reader backgrounds and objectives. (Chapter 2)

**reference sweeping** -- The O(N) operation performed after task deletion that removes references to the deleted task from all other tasks' blocks and blockedBy arrays, maintaining referential integrity. Reference sweeping prevents dangling references that could cause incorrect task scheduling. (src/tools/TaskDeleteTool/)

**resolve-once guard** -- An atomic check-and-mark primitive (createResolveOnce) that prevents multiple concurrent racers from resolving the same permission promise, using a claimed boolean to close the window between checking isResolved() and calling resolve(). The resolve-once guard prevents race conditions in concurrent permission evaluation. (src/hooks/useCanUseTool.tsx)

**safe-properties auto-allow** -- The optimization in SkillTool.checkPermissions that auto-allows skills whose only non-trivial properties are in the SAFE_SKILL_PROPERTIES set, reducing permission fatigue for safe skills. Safe-properties auto-allow reduces the number of permission dialogs for well-behaved skills. (src/tools/SkillTool/)

**safety-check decision reason** -- A permission decision reason type (decisionReason.type === 'safetyCheck') that makes a tool immune to bypassPermissions and auto-mode classifiers, used for cross-machine communication in SendMessage. Safety-check decisions represent the highest trust level in the permission hierarchy. (src/tools/SendMessageTool/)

**scale confusion** -- The conflation of harnesses operating at very different complexity levels (bash loop ~10 lines, generator-evaluator ~100s, full orchestration ~1000s, reference architecture aspirational) as if they were points on a single continuum. Scale confusion leads to inappropriate generalization of results across very different system types. (HER §18)

**screen differencing** -- The technique of comparing two screen buffers cell-by-cell to emit minimal terminal updates, used in cc's double-buffered rendering pipeline. Screen differencing reduces terminal output volume, improving rendering performance on slow connections. (src/ink/screen.ts)

**scroll drain** -- A mechanism that suspends background intervals during active terminal scrolling to prevent scroll jank, with a 150ms debounce timer that auto-clears. Scroll drain prioritizes user-perceived responsiveness over background task updates. (src/ink/)

**search hint** -- A curated, high-signal capability phrase on a tool definition (e.g., 'search file contents with regex (ripgrep)') that scores higher than the full description in ToolSearchTool keyword matching. Search hints bridge the gap between user intent and tool discovery. (src/tools/ToolSearchTool/)

**section size budget** -- The dual constraint (MAX_SECTION_LENGTH per section, MAX_TOTAL_SESSION_MEMORY_TOKENS total) that prevents session memory from consuming excessive post-compact context. Section size budgets enforce the HER principle of bounded context, preventing any single memory section from monopolizing the context window. (src/services/SessionMemory/)

**selector discipline** -- The practice of ensuring useAppState selectors return stable references (existing sub-object references) rather than creating new objects, to prevent infinite re-renders in useSyncExternalStore-backed hooks. Selector discipline is a performance invariant that prevents reactive loops. (src/state/)

**semantic type wrapper** -- A Zod schema wrapper (semanticNumber, semanticBoolean) that coerces string-typed fields from LLM JSON output into their proper types, reducing tool-call failures from type mismatches. Semantic type wrappers handle the common case where LLMs emit numeric and boolean values as strings. (src/Tool.ts)

**sensor** -- An observability input that feeds runtime state (cost, context size, error rates) back into the harness for adaptive decisions. Sensors correspond to Martin Fowler's taxonomy of "sensors" -- feedback controls that measure system state and trigger responses. (HER §4)

**session** -- A single invocation of the agent from startup to shutdown, persisted as JSONL entries with tombstones for resumability. Sessions implement the HER Session Protocol (§10.3), with structured startup, execution, and shutdown phases. (src/utils/sessionStorage.ts)

**session hook registry** -- The Map<string, SessionStore> structure keyed by session ID that stores ephemeral, in-memory hooks (both command and function type) for the duration of a session. Session hooks enable dynamic hook registration that is automatically cleaned up when the session ends. (src/utils/hooks/sessionHooks.ts)

**session memory** -- A structured Markdown file automatically maintained by a forked subagent during a conversation, capturing task state, errors, and progress in fixed sections, used as input for session-memory compaction. Session memory implements the HER principle of structured handoff across context boundaries. (src/services/SessionMemory/)

**session stamping** -- The practice of re-stamping provenance fields (sessionId, cwd, entrypoint, version) after message spreads to prevent cross-session contamination in forked or resumed sessions. Session stamping is a safety mechanism that prevents session state from leaking across process boundaries. (src/utils/sessionStorage.ts)

**settings cascade** -- The five-source merge hierarchy (plugin -> user -> project -> local -> flag -> policy) that deep-merges configuration from multiple sources with later sources overriding earlier ones. The settings cascade implements the HER principle of layered configuration with predictable precedence. (src/utils/settings/)

**short message ID** -- A 6-character base36 hash derived from a message UUID for use in the history_snip compaction system. Short IDs provide human-readable references to messages while maintaining uniqueness within a session. (src/utils/messages.ts)

**sibling abort** -- The mechanism by which a Bash tool error cancels sibling concurrent tool executions via a child abort controller, without aborting the parent query. Sibling abort implements fail-fast semantics for parallel tool execution, preventing wasted work after a failure. (src/services/tools/toolExecution.ts)

**side query** -- A secondary API call using a smaller model (typically Sonnet) to evaluate or select items without consuming the main model's context budget, used in memdir for relevance selection. Side queries implement model routing, matching task complexity to model cost. (src/memdir/findRelevantMemories.ts)

**skill** -- A reusable, frontmatter-defined capability discovered from ~/.claude/skills or bundled, invoked inline or in a forked context. Skills implement HER §7.3 (Progressive Disclosure), enabling the agent to discover and activate new capabilities dynamically. (src/skills/)

**slash command** -- A user-invocable command parsed from the REPL input line, registered in commands.ts as either prompt or callback type. Slash commands are a configuration surface that enables user-facing functionality without modifying the agent's core behavior. (src/commands.ts)

**slug validation** -- The process of checking a user-controlled string against strict character-allowlist and length rules before joining it into a filesystem path, preventing directory-escape and path-traversal attacks. Slug validation is a structural control that makes path injection impossible by construction. (src/utils/worktree.ts)

**speculative classifier check** -- A pattern where a bash classifier result is raced against a timeout during the permission decision flow, allowing auto-approval of common commands without showing a dialog. Speculative checks reduce perceived latency for clearly safe operations while falling back to interactive prompts for ambiguous cases. (src/hooks/useCanUseTool.tsx)

**spend-rate monitor** -- A rolling-average anomaly detector that fires alerts when both cost-per-minute and tokens-per-minute exceed 2x their respective rolling averages. The spend-rate monitor detects cost anomalies earlier than absolute cost caps, enabling intervention before budgets are exhausted. (src/query/)

**SSRF guard** -- The dns.lookup-compatible function in src/utils/hooks/ssrfGuard.ts that blocks connections to private, link-local, and CGNAT address ranges, preventing HTTP hooks from reaching cloud metadata endpoints. The SSRF guard is a critical security control that prevents the agent from accessing internal network resources. (src/utils/hooks/ssrfGuard.ts)

**staleness warning** -- A text injection appended to recalled memories older than one day via memoryFreshnessText(), reminding the model to verify claims before asserting them as fact. Staleness warnings implement the HER principle of self-aware context quality, preventing the agent from relying on outdated information. (src/memdir/)

**state destructuring pattern** -- The pattern where mutable State is destructured at the top of each loop iteration and reassigned at each continue site, making state transitions explicit and testable. State destructuring enables formal reasoning about the query loop's state machine. (src/query.ts)

**stop hook** -- A user-defined hook evaluated at the Stop lifecycle event when the model declares completion, allowing the harness to inject continuation messages and prevent premature completion. Stop hooks implement HER Failure Mode 6.2 (Premature Completion) by providing a structural control against the agent declaring done too early. (src/query/stopHooks.ts)

**Stone Soup attribution** -- The misattribution of massive human engineering effort to AI capability, where specification, monitoring, and correction labor is invisible in productivity metrics. Stone Soup attribution leads to overestimation of autonomous agent capability and underinvestment in harness engineering. (HER §18)

**structural control** -- An architectural constraint in the harness that makes certain failure modes structurally impossible, as opposed to advisory controls (guides) or feedback controls (sensors). cc's permission system and tool dispatch pipeline are examples. (HER §4)

**structural sharing** -- The pattern where setState creates a new root object while sharing unchanged sub-trees by reference, enabling Object.is identity checks for efficient re-render skipping in reactive state systems. Structural sharing provides O(1) identity checks for unchanged state subtrees. (src/state/)

**structured handoff** -- The pattern of writing a progress file before clearing context and resuming from the file, used as the escape hatch for compaction failure and session-to-session transition. Structured handoffs implement HER Pattern 1, ensuring that no information is lost when context is reset. (HER §5)

**structured message protocol** -- A discriminated-union message schema for inter-agent communication supporting typed lifecycle events (shutdown_request, shutdown_response, plan_approval_response) with request IDs for correlation. The structured message protocol enables reliable inter-agent communication with request-response semantics. (src/tools/SendMessageTool/)

**subagent** -- An agent spawned by the parent agent via AgentTool, running in sync, forked, or remote mode with isolated context. Subagents implement HER Pattern 7 (Context-Isolated Subagents), enabling the parent to delegate work without contaminating its own context window. (src/tools/AgentTool/)

**synthetic message** -- A harness-generated message that never originates from the user or the model, used for flow control (INTERRUPT_MESSAGE, CANCEL_MESSAGE, REJECT_MESSAGE, NO_RESPONSE_REQUESTED). Synthetic messages enable the harness to inject control signals into the message stream without impersonating the user or the model. (src/utils/messages.ts)

**task** -- A durable unit of work tracked through status transitions (pending, in_progress, completed), stored on disk with filesystem locking. Tasks implement the HER principle of persistent state tracking, ensuring that work items survive process crashes and context resets. (src/utils/tasks.ts)

**task budget** -- An API-level budget for the whole agentic turn (output_config.task_budget), distinct from the client-side token budget, with remaining computed per iteration from cumulative API usage. Task budgets provide a server-side cost control that complements the client-side token budget. (src/query/tokenBudget.ts)

**task list identity** -- The mechanism by which getTaskListId() determines which task list an agent operates on, using a priority chain: environment variable, teammate context, team name, leader team name, or session ID. Task list identity enables multi-agent teams to share a common task list. (src/utils/tasks.ts)

**three-gate cascade** -- A pattern for background task scheduling where progressively more expensive checks (time, workload, lock) are evaluated in order, minimizing wasted computation when upstream gates would have rejected the task. The three-gate cascade implements short-circuit evaluation for background process scheduling. (src/services/autoDream/)

**three-outcome model** -- The hook result contract where every hook execution produces one of three primary outcomes: success, blocking, or non_blocking_error (plus cancelled for external interruption). This is a core invariant of the hook system that simplifies error handling and ensures consistent behavior. (src/schemas/hooks.ts)

**token budget** -- A client-side limit on the number of tokens consumed per agentic turn, enforced by BudgetTracker and checkTokenBudget() to detect diminishing returns and budget exhaustion. Token budgets implement HER §13 (Cost Management) at the query-loop level. (src/query/tokenBudget.ts)

**token estimation pipeline** -- The three-tier counting system (API-based, Haiku fallback, rough heuristic) that provides token counts with decreasing accuracy and latency, used when the primary API endpoint is unavailable. The estimation pipeline ensures that token budgeting works even when exact counts are unavailable. (src/services/tokenEstimation.ts)

**tool** -- A typed, schema-validated operation exposed to the model via the tool dispatch pipeline, implemented as a Zod-defined input and a call() method. Tools implement HER Pattern 11 (Single-Purpose Tool Design), where each tool has a narrow, well-defined responsibility. (src/Tool.ts)

**ToolSearch** -- The progressive tool expansion system that exposes a catalog of available tools and loads them lazily upon model request. ToolSearch implements HER Pattern 9, trading a small discovery cost for significant prompt-size savings on every query. (src/tools/ToolSearchTool/)

**trace span** -- An OpenTelemetry-compatible observability record linking tool dispatches, subagent spawns, and model invocations into a causal trace tree. Trace spans enable distributed tracing across multi-agent workflows, implementing the HER principle of full observability. (src/services/analytics/)

**transcript projection** -- The security-motivated transformation of the raw conversation into a compact transcript that excludes assistant text to prevent classifier manipulation. Transcript projection implements the HER principle that the classifier should not see text that the model could have manipulated. (src/utils/permissions/yoloClassifier.ts)

**two-stage classifier** -- A classification pipeline that runs a fast low-token stage first and a slower thinking stage only when the fast stage flags a potential block, reducing latency for clearly safe actions. Two-stage classification optimizes the common case (safe actions) while preserving accuracy for ambiguous cases. (src/utils/permissions/yoloClassifier.ts)

**typeahead engine** -- The multi-source suggestion aggregation system in useTypeahead that merges slash commands, file paths, shell completions, Slack channels, and agent names into a unified ranked list with debouncing and stale-result filtering. The typeahead engine reduces user input latency by predicting likely completions. (src/hooks/useTypeahead.tsx)

**verification nudge** -- The advisory signal in TodoWrite and TaskUpdateTool that fires when the main-thread agent completes 3+ tasks without including a verification step, appending a reminder to spawn the verification subagent. Verification nudges implement the HER principle of verify-before-trust, preventing the agent from accumulating unverified work. (src/tools/TodoWriteTool/, src/tools/TaskUpdateTool/)

**work-secret authentication** -- A per-session authentication mechanism where each spawned cc session is bound to a unique work secret generated at spawn time, preventing unauthorized clients from connecting to or injecting messages into a running session. Work-secret authentication provides defense-in-depth for the bridge transport layer. (src/bridge/)

**worktree** -- A git worktree providing an isolated working directory for parallel development branches, managed by EnterWorktreeTool/ExitWorktreeTool. Worktrees implement HER Pattern 8 (Fork-Join Parallelism), enabling multiple agents to work on different branches simultaneously without interfering with each other. (src/tools/EnterWorktreeTool/, src/tools/ExitWorktreeTool/)

**YOLO classifier** -- The LLM-based classifier that evaluates tool actions against a security policy using a two-stage XML pipeline with transcript projection and prompt caching. The YOLO classifier provides semantic understanding of command intent that pattern-based classifiers cannot achieve. (src/utils/permissions/yoloClassifier.ts)
