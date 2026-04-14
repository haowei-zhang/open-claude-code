# Part II: Entry, Bootstrap, and the Runtime Spine

If Part I gave you the map, Part II walks you through the front door and down the central corridor. The theme of this Part is the runtime spine: the sequence of code that runs from the moment a user types `claude` at the shell prompt through to the steady-state loop that drives every interaction with the model.

These six chapters belong together because they trace a single, continuous execution path. Bootstrap sets up the process. The command router dispatches the user's intent. The query loop iterates between model calls and tool execution. The API layer manages the streaming connection to Anthropic's models. The prompt assembler composes the instructions that shape each model call. And the token-budget subsystem ensures the whole apparatus stays within its resource limits. Each chapter picks up where the previous one left off, and together they account for every millisecond and every allocation between the shell and the model's first token.

Chapter 5, "Bootstrap: From `bun run` to Running Loop," traces the startup sequence from `cli.tsx` through fast-path flag handling, model-discovery prefetch, keychain access, version checks, TLS and mTLS setup, policy limit enforcement, OAuth flows, and graceful shutdown registration. By the end of this chapter, the process is alive and ready to accept commands.

Chapter 6, "`main.tsx` and the Command Router," dives into the approximately 4,700-line entry point. It covers CLI subcommand parsing, the `renderAndRun` function, and the critical bifurcation between headless mode (for scripts and pipes) and interactive mode (for the terminal UI).

Chapter 7, "The Query Loop: Heartbeat of the Agent," provides an exhaustive walkthrough of `queryLoop()` in `src/query.ts`. This is the async generator protocol that bridges model streaming and tool execution -- the heartbeat that makes cc an agent rather than a chatbot. The chapter covers the `StreamEvent` and `Message` type hierarchy, tool dispatch points, interruption mechanisms, stop-hook evaluation, and token budget checks.

Chapter 8, "Talking to Anthropic: `services/api/claude.ts` and Streaming," examines the Anthropic SDK wrapper. Streaming, fallback model selection, token metering, retry logic, prompt-cache break detection, rate-limit handling, first-token latency tracking, and usage accumulation are all covered here.

Chapter 9, "System Prompts and Prompt Assembly," dissects `src/constants/prompts.ts` and the layered composition of the final system prompt for each query. Base instructions, environment context, tool descriptions, skill injections, memories, hooks, effort-level adjustments, and plan-mode directives are assembled into a single prompt that shapes every model call.

Chapter 10, "Token Budgets, Effort, and Fast Mode," explains how cc tracks and bounds token consumption, how the effort setting selects among model tiers (Opus, Sonnet, Haiku) and thinking budgets, and how fast mode flips the loop's behavior for lower-latency, lower-cost interactions.

By the end of Part II, the reader should understand the complete lifecycle of a single user interaction: how the process boots, how the command is routed, how the query loop iterates, how the model is called, how the prompt is composed, and how resource limits are enforced. This is the skeletal structure upon which every feature in the remaining Parts depends -- tools, agents, permissions, memory, and the terminal UI all plug into the query loop and the prompt assembler described here.

The table of contents in the front matter provides the full chapter listing and page numbers for the entire book.
