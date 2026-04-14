# Glossary

**Agent** -- A software system composed of a language model plus a harness. The equation Agent = Model + Harness is a pedagogical simplification: in practice, the harness itself may contain model calls (evaluator agents, summarization agents), making the relationship recursive. See Chapter 1.

**Agent Definition** -- A markdown file with YAML frontmatter that specifies a subagent's name, model, prompt template, and behavioral constraints. Discovered from `~/.claude/agents/`, plugin directories, or bundled defaults. See Chapter 20.

**Agent Tool** -- The primary tool (`src/tools/AgentTool/`) for invoking subagents. Accepts inputs including description, prompt, subagent_type, model, run_in_background, team_name, mode, isolation, and cwd. Validates inputs and selects an execution mode (sync, fork, or remote). See Chapter 18.

**Ashby's Law** -- The principle that "a regulator must have at least as much variety as the system it governs." Applied to harness engineering, this means committing to defined topologies narrows the variety space, making comprehensive harnesses achievable. (HER S4.6)

**Autocompact** -- Automatic compaction triggered when the context window exceeds a configurable threshold. Uses the model itself to generate a summary of the conversation so far, then replaces older messages with that summary. Implemented in `src/services/compact/autoCompact.ts`. See Chapter 28.

**Back-Pressure** -- A mechanism that slows or stops the agent's loop when resource limits are approached, preventing unbounded consumption. In cc, applies to token budgets, cost caps, and rate limits. In HER's framing, back-pressure also describes the principle of feeding back only failures (errors) to the agent while suppressing verbose success output. (HER S7.6) See Chapters 10, 28.

**Bash Classifier** -- A function in `src/utils/permissions/bashClassifier.ts` that maps a bash command string to a risk band (safe, risky, destructive) to determine whether automatic approval is appropriate. Part of cc's command risk classification system. See Chapter 33.

**Checkpoint-Restore** -- The ability to save and restore agent state at a known-good point. HER identifies side effects (filesystem changes, API calls, credential reuse) as the primary obstacle to correct restoration. The ACRFence paper (arXiv:2603.20625) identifies two attack classes: Action Replay and Authority Resurrection. (HER S6.16) See Chapters 29, 52.

**CLAUDE.md** -- A persistent instruction file auto-loaded at session start, following the format recommended by HER Pattern 1. Contains project conventions, coding standards, and workflow instructions. Loaded as part of the scoped context assembly cascade. The ETH Zurich study found that verbose instruction files can reduce task success rates, recommending minimal targeted context instead. (HER S7.1) See Chapters 9, 31.

**Classifier** -- A function that maps a bash command string to a risk band (safe, risky, destructive) to determine whether automatic approval is appropriate. Implemented in `src/utils/permissions/bashClassifier.ts`. The YOLO classifier extends this with a numeric scoring approach. See Chapter 33.

**Compaction** -- The process of reducing context size by summarizing or removing older messages. cc implements a five-stage hierarchy: history_snip, microcompact, context collapse, autocompact, and hard reset. Each stage has different triggers, preservation semantics, and discard semantics. See Chapter 28.

**Compounding Bugs Across Sessions** -- A failure mode where a new session builds on broken state from a previous session. The fix is baseline verification at session start before any new work begins. (HER S6.9) See Chapter 54.

**Computational Controls** -- Deterministic, fast (millisecond) harness controls such as tests, linters, and type checkers. Reliable but limited in scope compared to inferential controls. (HER S4.2) See Chapter 53.

**Context Anxiety** -- A failure mode where models prematurely wrap up work near perceived context limits, even when substantial budget remains. The fix is context resets with structured handoffs rather than pushing to the limit. (HER S6.5) See Chapter 54.

**Context Collapse** -- A severe compaction event where large portions of the conversation are replaced with a summary. Triggered when token usage approaches model limits. More aggressive than microcompact but less drastic than a hard reset. See Chapter 28.

**Context Rot** -- Performance degradation of 30% or more when key content falls in mid-window positions. The agent re-solves previously completed problems, contradicts itself, and loses track of goals. The fix is active context management through summarization, pruning, and the progress file pattern. (HER S6.1) See Chapters 7, 28, 54.

**Context Window** -- The token budget available to the model in a single request. Analogous to "perception" in the academic literature: the filesystem is long-term memory, RAM is working memory, and the context window is what the agent can currently "see." The harness manages what enters and exits perception. (HER S8.3) See Chapter 28.

**Cost Explosion** -- A failure mode where agents in infinite loops or multi-agent chains accumulate excessive token costs. Token costs in multi-agent systems compound non-linearly. Enterprise budgets underestimate AI agent total cost of ownership by 40-60%. The fix includes per-session cost caps, real-time token metering, and anomaly detection. (HER S6.11) See Chapters 10, 50, 54.

**Data Leakage Between Contexts** -- A failure mode where information leaks between sessions, between subagents, or through file-based communication. File-based communication inherently persists data to disk, and without cleanup, sensitive data from one task context can bleed into another. (HER S6.15) See Chapters 34, 54.

**Deferred Tool** -- A tool that is not loaded into the model's tool list at session start but is discovered on-demand via ToolSearch. Implements HER Pattern 9 Progressive Tool Expansion. MCP tools are commonly deferred to avoid the tool explosion problem. See Chapter 15.

**Dream Consolidation** -- A background process that reviews, deduplicates, and prunes memory during idle time. In cc, implemented via DreamTask and the autoDream service with a consolidation lock to prevent concurrent writes. (HER Pattern 4) See Chapter 24.

**Effort Level** -- A runtime parameter that selects the model tier (Opus, Sonnet, Haiku) and thinking budget for a query. Higher effort selects more capable models with longer thinking; lower effort trades accuracy for speed and cost. See Chapter 10.

**Explore-Plan-Act Loop** -- A three-phase workflow pattern with escalating permissions: read-only exploration, discussion and planning, then full-access implementation. In cc, implemented as plan mode V2 with five phases and enforced tool gating. (HER Pattern 6) See Chapter 45.

**Fork** -- A subprocess-based subagent execution mode where the agent runs in a forked process with isolated state, communicating via structured IPC. Implemented in `src/utils/forkedAgent.ts`. Contrasted with in-process sync agents and remote agents. See Chapter 19.

**Fork-Join Parallelism** -- A pattern where multiple subagents execute in isolated git worktrees with cached parent context reuse, then merge their results. In cc, the EnterWorktreeTool and ExitWorktreeTool manage worktree creation and teardown. (HER Pattern 8) See Chapter 46.

**Generator-Evaluator Pattern** -- A multi-agent orchestration pattern where one agent generates solutions and another evaluates them. The evaluator operates in a fresh context window with no knowledge of the generation process. Identified in HER as the most universally recommended pattern, but with an Ouroboros risk: when both are LLMs, self-reinforcing errors can amplify rather than correct. (HER S9.2) See Chapter 21.

**Goal Misinterpretation** -- A failure mode where the agent optimizes for proxy metrics rather than actual intent. Distinct from premature completion in that the agent may do significant wrong work rather than stopping early. The fix is explicit acceptance criteria with concrete examples and human checkpoints. (HER S6.17) See Chapter 54.

**Greenfield** -- A new codebase where the agent has maximum freedom and minimal legacy constraints. Most canonical harness patterns were designed for greenfield projects. Brownfield (existing codebase) work is significantly harder and requires extended orientation phases and stricter scope constraints. (HER S10.5) See Chapter 55.

**Guide** -- A harness component that shapes the agent's behavior through prompts, CLAUDE.md files, skill templates, and system instructions. One of Martin Fowler's two primary control mechanisms (the other being sensors). Guides are feedforward controls that anticipate and prevent unwanted behavior before it occurs. (HER S4.1) See Chapter 53.

**Hallucinated Tool Calls** -- A failure mode where the agent fabricates tool parameters, calls wrong APIs, or reports success on actions that silently failed. Distinct from silent failures in that the agent invents tool calls that look structurally valid but are semantically wrong. The fix is schema validation on all tool call parameters and semantic validation for critical operations. (HER S6.12) See Chapter 54.

**Harness** -- The engineering layer surrounding a language model that provides tools, context assembly, permission enforcement, lifecycle hooks, and state management. The term was popularized by Mitchell Hashimoto in February 2026. A harness encompasses every piece of code, configuration, and execution logic that is not the model itself. See Chapter 1.

**Harness Engineering** -- The practice of designing, building, and maintaining the complete software infrastructure that wraps around an AI model to make it capable of reliable, autonomous work. The discipline encompasses prompt engineering and context engineering as nested layers. TerminalBench 2.0 data shows a 24.9 percentage point spread across harnesses using the same model, proving that harness design matters more than model selection for production reliability. (HER S1) See Chapter 1.

**Harnessability** -- A measure of how well a codebase supports harness implementation. Factors that improve harnessability include strongly typed languages, clear module boundaries, and conventional frameworks. Legacy systems with technical debt face particular challenges: harnesses are most necessary where they are hardest to build. (HER S4.5) See Chapter 53.

**History Snip** -- The first and lightest stage of the compaction hierarchy. Marks older messages with a `<history_snip>` tag to signal that they have been truncated, preserving the structural outline of the conversation while removing detailed content. See Chapter 28.

**Hook** -- A deterministic lifecycle event handler registered in `settings.json`. Hook types include command, prompt, http, agent, and function. Fires on PreToolUse, PostToolUse, SessionStart, and other lifecycle events. Defined by `src/schemas/hooks.ts`. (HER Pattern 12) See Chapter 36.

**Human-in-the-Loop (HITL)** -- Design patterns for integrating human judgment into agent workflows. Patterns include confidence-based routing, tiered escalation, async approval, context-rich escalation, and handoff protocols. The AskUserQuestion tool implements HITL in cc by pausing the loop for human input. (HER S11) See Chapter 17.

**Infinite Loops** -- A failure mode where the agent retries without making progress. The fix includes maximum retry counts, exponential backoff, and loop detection. In cc, stop hooks and token budget checks serve as loop detection mechanisms. (HER S6.7) See Chapter 7.

**Inferential Controls** -- AI-powered semantic analysis controls that are slower and non-deterministic than computational controls but richer for complex judgments. Contrasted with computational controls in Martin Fowler's taxonomy. (HER S4.2) See Chapter 53.

**JSONL Session** -- The on-disk format for persisting a cc session. Each line is a JSON entry with a type field and optional tombstones for deleted entries. Supports head/tail reading for efficient resume. Managed by `src/utils/sessionStorage.ts`. See Chapter 29.

**Manifest** -- The build registry file (`.book-build/manifest.json`) containing every chapter's metadata, status, pass counts, and quality metrics. The single source of truth for build state in the book's construction pipeline.

**Memdir** -- The tiered on-disk memory system at `~/.claude/projects/<project>/memory/`. Contains a `MEMORY.md` index (200 lines max, always in context) and per-memory `.md` files loaded on demand. Implements HER Pattern 3 Tiered Memory. See Chapter 26.

**Memory** -- A persistent knowledge record stored in the memdir filesystem. Types include user, feedback, project, and reference. Each has a frontmatter schema with name, description, and type fields. Memory is extracted after sessions and consolidated during dream tasks. See Chapters 26, 27.

**Microcompact** -- The second stage of the compaction hierarchy. Selectively summarizes individual messages or message groups while preserving key information such as tool calls and results. Less disruptive than context collapse. Implemented in `src/services/compact/microCompact.ts`. See Chapter 28.

**Model Regression** -- A failure mode where a provider updates a model and harness behavior breaks silently because the harness was tuned for specific model behaviors that changed. The fix is regression test suites for the harness, pinning model versions where stability matters, and monitoring key metrics after provider updates. (HER S6.14) See Chapters 8, 54.

**MCP (Model Context Protocol)** -- A protocol for extending cc with external tool servers. Transports include stdio and SSE. MCP servers have a connection lifecycle managed by `src/services/mcp/MCPConnectionManager.tsx`. MCP tools are commonly deferred to avoid tool explosion. MCP is also a supply chain attack surface requiring vetting. (HER S7.2) See Chapters 40, 41.

**Observation Masking** -- The practice of replacing verbose tool outputs with compressed summaries before feeding them back into the model context. HER reports 52% cost reduction with this technique (JetBrains Research, verified). The principle is to "swallow the output and only surface errors." (HER S8.1) See Chapter 28.

**One-Task-Per-Session Rule** -- The single most impactful rule across all harness engineering sources. Prevents context exhaustion mid-feature, scope creep, compounding errors, and lost progress due to context resets. Multi-hour tasks must be decomposed into many single-task sessions with state handoffs between them. (HER S10.1) See Chapter 22.

**Ouroboros Problem** -- The risk that when both generator and evaluator are LLMs, self-reinforcing errors amplify rather than correct. If the evaluator hallucinates approval, the feedback loop validates bad output. Mitigations include using deterministic sensors as the primary quality gate and grounding evaluator output in observable artifacts. (HER S9.2) See Chapter 21.

**Permission Mode** -- A runtime mode governing how tool-use requests are authorized. Modes include default (ask), plan (read-only), acceptEdits, bypassPermissions, dontAsk, and auto. Defined in `src/utils/permissions/PermissionMode.ts`. Each mode determines which tools require explicit human approval. See Chapter 32.

**Placeholder Implementations** -- A failure mode where agents default to stub implementations because compiling triggers reward signals. The fix is explicit anti-placeholder instructions in prompts, such as requiring full implementations with no simplified or stub code. (HER S6.4) See Chapter 54.

**Plan Mode** -- A read-only exploration phase where the agent can read files and search but cannot write or execute tools. cc implements plan mode V2 with five phases and enforced tool gating. Part of the Explore-Plan-Act loop pattern. Implemented in `src/utils/planModeV2.ts`. (HER Pattern 6) See Chapter 45.

**PostToolUse** -- A lifecycle hook event that fires after a tool has executed. Can inspect and modify the tool result before it reaches the model. Part of the hook schema in `src/schemas/hooks.ts`. (HER Pattern 12) See Chapter 36.

**PreToolUse** -- A lifecycle hook event that fires before a tool is executed. Can block, modify, or approve the tool call before execution begins. Part of the hook schema in `src/schemas/hooks.ts`. (HER Pattern 12) See Chapter 36.

**Premature Completion** -- A failure mode where the agent declares work done too early, often because it satisfies a shallow interpretation of the task requirements. The fix is comprehensive JSON feature lists with all items initially marked as failing, forcing the agent to verify each one. (HER S6.2) See Chapter 54.

**Prompt Assembly** -- The process of composing the final system prompt per query from multiple sources: base system prompt, environment information, tool descriptions, skill templates, memory records, hook outputs, effort level, and plan mode directives. Implemented in `src/constants/prompts.ts` and related modules. (HER Pattern 2) See Chapter 9.

**Progressive Context Compaction** -- A multi-stage system for reducing context size at configurable token thresholds. In cc, the stages are history_snip, microcompact, context collapse, autocompact, and hard reset. Each stage preserves progressively less detail. (HER Pattern 5) See Chapter 28.

**Progressive Tool Expansion** -- A pattern where the agent starts with fewer than 20 tools and activates more on demand, rather than exposing all 60+ tools at once. Excessive tool descriptions push agents into degraded selection accuracy. Implemented in cc via ToolSearch and deferred tool loading. (HER Pattern 9) See Chapter 15.

**Query Loop** -- The core execution loop in `src/query.ts` that bridges model streaming and tool execution. An async generator protocol that yields StreamEvent and Message types, handles tool dispatch points, interruption signals, and stop-hook evaluation. The heartbeat of the agent. See Chapter 7.

**Ralph Wiggum Pattern** -- Geoffrey Huntley's iterative eventual consistency approach: a Bash loop that repeatedly pipes a plan document into the agent. Relies on one-thing-per-loop discipline, deterministic stack allocation, and trust in the model to decide priorities. Two phases: generate (cheap, controlled by specs) and back-pressure (verify via type systems, linters, tests). (HER S3.4) See Chapter 1.

**Risk Band** -- A classification level assigned to a bash command by the classifier. The three bands are safe (auto-approved), risky (requires confirmation), and destructive (requires explicit approval even in auto mode). Implemented in `src/utils/permissions/bashClassifier.ts`. See Chapter 33.

**Scoped Context Assembly** -- Multi-level instruction loading that assembles context from org, user, project, directory, and environment layers. Each layer adds scope without eliminating the previous. Implements HER Pattern 2. (HER S7.1) See Chapter 9.

**Self-Evaluation Bias** -- A failure mode where agents confidently praise their own work even when quality is mediocre. The fix is separating generator from evaluator agents in fresh context windows to create external feedback loops. (HER S6.3) See Chapter 54.

**Sensor** -- A harness component that observes agent behavior and feeds observations back into the context. Examples include file-read results, command outputs, debug logs, and test suite results. One of Martin Fowler's two primary control mechanisms (the other being guides). Sensors are feedback controls that enable self-correction after generation. (HER S4.1) See Chapter 53.

**Session** -- A single invocation of cc from startup to shutdown. Persisted as JSONL with entry types and tombstones. Supports resume via head/tail reading. Managed by `src/utils/sessionStorage.ts`. (HER S10.3) See Chapter 29.

**Session Protocol** -- A structured sequence for multi-session work: ORIENT (read workspace state), SETUP (run init script), VERIFY (run baseline tests), SELECT (pick one task), IMPLEMENT (write code), TEST (run automated checks), UPDATE (persist state), EXIT (clean shutdown). Synthesized across Anthropic, OpenAI, and practitioner sources. (HER S10.3) See Chapter 5.

**Settings Cascade** -- The loading order for configuration sources: org, user, project, directory, environment, and CLI flags. Later sources override earlier ones. Managed by `src/utils/settings/settings.ts` and related modules. (HER S7) See Chapter 31.

**Silent Failures** -- A failure mode where the agent proceeds after tool errors as if the operation succeeded. The fix is structured output validation after every tool call. (HER S6.6) See Chapter 54.

**Skill** -- A reusable prompt template discovered from `~/.claude/skills/` or bundled, with YAML frontmatter metadata. Can be user-invocable (slash command) or triggered. Supports inline and fork context modes. Part of the progressive disclosure configuration surface. (HER S7.3) See Chapter 38.

**Slash Command** -- A user-typed command starting with `/` that is dispatched through the command registry in `src/commands.ts`. Can be a prompt template or a callback function. Part of the configuration surfaces that shape agent behavior. (HER S7) See Chapter 39.

**SSRF Guard** -- A security mechanism in `src/utils/hooks/ssrfGuard.ts` that prevents the agent from making HTTP requests to internal network addresses. Applied to web tools and HTTP hooks to prevent server-side request forgery attacks. See Chapter 37.

**Subagent** -- A single-use agent invoked by the parent session via the Agent tool. Has a fresh context window, returns one result, and does not persist across dispatches. (HER S7.4) See Chapter 18.

**Task** -- A durable unit of work with a status lifecycle (pending, in_progress, completed), blocking relationships, filesystem locking, and one of seven task types. Recorded in `src/utils/tasks.ts`. The one-task-per-session rule is the most impactful design constraint across all harness engineering sources. (HER S10.1) See Chapter 22.

**Three-File State Pattern** -- A pattern for state persistence across sessions using three files: a structured task list (`tasks.json`), a human-readable progress file (`progress.txt`), and a discovered-patterns file (`AGENTS.md`). Combined with git history, these provide complete session-to-session continuity. (HER S8.2) See Chapter 22.

**Tiered Memory** -- A memory architecture where a compact index is always in context, topic files are loaded on demand, and full transcripts remain on disk. In cc, implemented as the memdir system with `MEMORY.md` as the index. (HER Pattern 3) See Chapter 26.

**Tool** -- A single-purpose capability exposed to the model via the tool dispatch pipeline. Each tool has a Zod/JSONSchema input schema, a `call()` method, concurrency flags, and optional deferral. Registered in `src/Tool.ts`. (HER Pattern 11) See Chapter 11.

**Tool Dispatch Pipeline** -- The sequence a requested tool call travels through: validateInput, canUseTool, runPreToolUseHooks, tool.call, runPostToolUseHooks, and result normalization. Implements read-only parallelism and write serialization. See Chapter 12.

**Tool Explosion** -- A failure mode where too many tools degrade the model's selection accuracy. The fix is progressive tool expansion starting with fewer than 20 tools and activating more on demand. (HER S6.8) See Chapter 15.

**ToolSearch** -- The tool-discovery mechanism that allows the model to search and load tools on demand rather than having all tools present in the initial tool list. Implemented in `src/tools/ToolSearchTool/`. Enables progressive tool expansion and deferred loading of MCP tools. (HER Pattern 9) See Chapter 15.

**Token Budget** -- The tracking and bounding mechanism for token consumption during a session. Implemented in `src/query/tokenBudget.ts`. Monitors input tokens, output tokens, and total cost against configurable limits. When budgets are approached, triggers compaction or loop termination. See Chapter 10.

**Worktree** -- A git worktree created as an isolated working directory for parallel agent execution, with its own branch and symlinked `node_modules`. Managed by EnterWorktreeTool and ExitWorktreeTool. Provides filesystem isolation for fork-join parallelism. (HER Pattern 8) See Chapter 46.

**Yak Shaving** -- A failure mode where the agent wanders into tangential fixes instead of completing the primary task. Also known as scope creep. The fix is strict single-task-per-session constraints and explicit task boundaries. (HER S6.10) See Chapter 54.
