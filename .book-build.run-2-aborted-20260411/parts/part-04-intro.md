# Part 4: Sub-Agents, Tasks, and Multi-Agent Dispatch

A single agent loop, no matter how sophisticated, hits a ceiling. The model can hold only so much context. The tool set grows until discovery itself becomes a bottleneck. The user's request may decompose into independent subproblems that cry out for parallel execution. Part 4 confronts the architectural leap from one loop to many -- from a solitary agent reasoning in a single conversation thread to a coordinated ensemble of sub-agents, tasks, and background processes that divide work, isolate context, and recombine results.

This part groups seven chapters around a single theme: how cc implements multi-agent orchestration atop its single-agent query loop. The chapters are arranged in dependency order. They begin at the surface -- the AgentTool API that a parent agent invokes -- and descend through execution modes, agent definitions, the swarm coordinator, the durable task substrate, in-process collaboration, and finally the dream loop that consolidates learning in the background.

Chapter 18, "The Agent Tool: Entry, Inputs, and Lifecycle," opens with the AgentTool surface itself: the input schema (description, prompt, subagent_type, model, run_in_background, team_name, mode, isolation, cwd), validation rules, and the contract by which a sub-agent returns output to its parent.

Chapter 19, "Sync vs Fork vs Remote: The Execution Modes," maps the three tiers of sub-agent execution -- in-process synchronous agents, forked subagents with isolated state, and remote CCR agents that run on cloud infrastructure -- and explains the tradeoffs in shared versus isolated state and communication paths across these tiers.

Chapter 20, "Agent Definitions: Loading, Frontmatter, and Built-Ins," describes how cc discovers and loads agents from ~/.claude/agents directories, plugin agents, and built-in definitions, all expressed through a Markdown-plus-YAML frontmatter contract that separates human-readable documentation from machine-parseable configuration.

Chapter 21, "Multi-Agent Coordinator: The Swarm Layer," dives into the feature-gated coordinatorMode.ts, the orchestration engine that manages multi-agent swarms, routes messages between teammates, and governs the lifecycle of a team from creation to shutdown.

Chapter 22, "Tasks: A Durable Unit of Work," introduces the task record -- the persistent, filesystem-backed substrate that underpins every agent run. This chapter covers status transitions, blocking relationships, filesystem locking, storage paths, high-water marks, and the seven distinct task types that cc defines.

Chapter 23, "Teammates and In-Process Collaboration," examines the tmux-based teammate system and in-process teammate tasks, focusing on the SendMessageTool for inter-agent communication and the idle hooks that keep teammates responsive without burning context.

Chapter 24, "Dream Tasks: Background Consolidation," closes the part with DreamTask and autoDream -- the background memory consolidation loop that fires when conditions align, extracting and compressing what the agent has learned so that future sessions start with sharper priors.

These chapters belong together because they form a single dependency chain: the AgentTool is the surface, execution modes determine isolation, definitions configure the agent, the coordinator orchestrates multiple agents, tasks persist their work, teammates communicate, and dreams consolidate the results. Remove any link and the chain breaks.

By the end of this part, the reader should understand how cc scales from a single query loop to a fleet of cooperating agents. The key takeaway is that multi-agent orchestration in cc is not a monolithic subsystem; it is a layered architecture where each layer -- tool surface, execution mode, coordination, persistence, communication, consolidation -- can be understood, tested, and extended independently. The reader should also appreciate the tension that runs through all seven chapters: the tradeoff between isolation (safety, reproducibility) and sharing (efficiency, coordination), and how cc navigates it at every tier.

Return to the table of contents in the front matter for the full chapter listing and cross-references to HER patterns and failure modes.
