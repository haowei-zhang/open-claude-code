# Part II. Entry, Bootstrap, and the Runtime Spine

Every long-running agent has a spine: the code path that starts when the process launches, routes the user's intent into a query, sends that query to the model, receives the streamed response, and loops back for the next turn. Part II traces that spine from first instruction to final iteration.

The theme of Part II is the runtime loop -- the heartbeat of the agent. These six chapters cover the entire lifecycle of a single query iteration, from the moment the user types a command to the moment the agent decides whether to continue or stop. They are grouped together because they form a sequential pipeline: bootstrap initializes the system, the command router dispatches, the query loop orchestrates, the API layer communicates with Anthropic, the prompt assembler composes the system prompt, and the token budget enforces cost and effort constraints. Each stage depends on the one before it, and together they constitute the core engine that powers every cc session.

Understanding this pipeline is essential because it is the path that every subsequent subsystem plugs into. The tool system (Part III) is invoked from within the query loop. The permission system (Part VI) gates tool dispatches that originate in the loop. The memory system (Part V) provides context that the prompt assembler injects. If you do not understand how the query loop works, you cannot understand how any of these subsystems are triggered or how they interact.

Chapter 5 traces the bootstrap sequence from `bun run` through fast-path flags, MDM prefetch, keychain prefetch, version check, TLS, policy limits, mTLS, OAuth, and graceful shutdown registration. This chapter answers the question: what happens between the moment you type `claude` and the moment the REPL appears? The answer involves a surprisingly long sequence of side effects, each of which can fail and each of which must complete before the agent can accept its first query.

Chapter 6 deep-dives into `main.tsx`, the approximately 4,700-line command router that handles CLI subcommands, parsing, `renderAndRun`, and the bifurcation between headless and interactive modes. This is the file that ties everything together -- it imports nearly every subsystem and orchestrates the handoff from bootstrap to the running loop.

Chapter 7 provides an exhaustive walkthrough of `queryLoop()` in `src/query.ts`, covering the async generator protocol, StreamEvent versus Message types, tool dispatch points, interruption points, and how the loop bridges model streaming and tool execution. This is the single most important function in the codebase: it is the loop that runs on every turn, and understanding it is prerequisite for understanding every tool dispatch, every permission check, and every compaction event.

Chapter 8 examines the Anthropic SDK wrapper in `services/api/claude.ts`, including streaming, fallback model selection, token metering, retries, prompt cache break detection, rate limits, and usage accumulation. This chapter covers the interface between cc and the model provider -- the layer that translates cc's query into an API request and streams the response back.

Chapter 9 covers `src/constants/prompts.ts` and how cc composes the final system prompt per query from base, environment, tools, skills, memories, hooks, effort, and plan mode fragments. The system prompt is the single most important input to the model, and understanding how it is assembled from dozens of fragments is essential for understanding how cc controls the model's behavior.

Chapter 10 explains how cc tracks and bounds token consumption, how effort levels select model and thinking configurations, and how fast mode alters the loop behavior. Token budgets are the financial and computational constraint that shapes every aspect of the agent's behavior, from model selection to compaction triggers.

By the end of this Part, you should understand how a cc process starts, how a user command becomes a model query, how the streamed response is processed and looped, how the API layer handles failures and metering, how the system prompt is assembled from dozens of fragments, and how token budgets and effort settings constrain the entire pipeline. This is the foundation for understanding every subsystem that follows.

Return to the Table of Contents in the front matter for an overview of all ten parts and fifty-seven chapters.
