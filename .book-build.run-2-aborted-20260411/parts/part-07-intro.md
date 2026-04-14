# Part 7: Interaction Surfaces: Skills, Slash, MCP, UX

An agent that exists only as a query loop and a set of tools is invisible to everyone except the developer reading its source. The user needs surfaces to invoke it, configure it, extend it, and watch it work. External tools need protocols to register capabilities. IDEs need bridges to embed the agent into existing workflows. Part 7 addresses every point where the agent touches a human or an external system: skills that progressively disclose capabilities, slash commands that give the user a keyboard-driven interface, the Model Context Protocol that lets third-party servers inject tools and resources, and the terminal rendering engine that makes all of it visible.

This part groups seven chapters around a single theme: the interaction surfaces through which users, external tools, and IDEs reach the agent. The chapters proceed from configuration surfaces (skills, slash commands) through integration surfaces (MCP) through presentation surfaces (Ink renderer, REPL, typeahead, voice) through bridge surfaces (VS Code, JetBrains, web).

Chapter 38, "Skills: Discovery, Frontmatter, Progressive Disclosure," explains how cc discovers skills from directories and bundled sets, how frontmatter fields declare a skill's capabilities, how skills are partitioned into user-invocable versus triggered categories, and how inline versus fork context modes determine whether a skill shares or isolates its execution environment.

Chapter 39, "Slash Commands: 90+ Commands and the Registry," opens the commands.ts registry that defines every slash command, distinguishes prompt-type from callback-type commands, and handles user-defined commands with argument substitution patterns.

Chapter 40, "MCP: Clients, Transports, and Lifecycle," introduces the Model Context Protocol integration: the transport layer, server lifecycle management, reconnect strategies, OAuth authentication flows, and elicitation handling that lets MCP servers interact with the user.

Chapter 41, "MCP Tools: Passthrough Permissions and Deferred Loading," covers MCPTool, ListMcpResourcesTool, ReadMcpResourceTool, and McpAuthTool, explaining why MCP tools are deferred by default and how ToolSearch surfaces them on demand rather than flooding the tool catalog at startup.

Chapter 42, "The Ink Renderer and Terminal Engine," descends into the custom React reconciler built on Ink, the Yoga layout engine, output rendering, focus management, text selection, and ANSI handling that transform a React component tree into a rich terminal interface.

Chapter 43, "REPL, PromptInput, Typeahead, Voice, Keybindings," covers the REPL screen, the prompt composer, the typeahead system built on an approximately 212,000-line hook, voice integration through an approximately 99,000-line hook, the bridge/inbox for inter-component communication, and the keybinding context system that maps keystrokes to actions.

Chapter 44, "Bridges: VS Code, JetBrains, and Web," closes the part with the IDE bridge architecture: how trusted devices are established, how inbound attachments flow from the IDE to the agent, and how the bridge transport layers enable a VS Code or JetBrains extension to host a cc session with full round-trip communication.

These chapters belong together because they constitute every surface where something outside the agent reaches in. Skills and slash commands are how the user shapes behavior. MCP is how external tools inject capability. Ink and the REPL are how the agent renders itself. Bridges are how the agent lives inside an IDE. Each surface has its own discovery protocol, its own permission model, and its own rendering path, but they all converge on the same query loop and tool dispatch pipeline described in earlier parts.

By the end of this part, the reader should understand how cc presents itself to the world. The key insight is that every interaction surface is a configuration surface: skills configure capabilities, slash commands configure invocation, MCP configures integration, and the terminal UX configures perception. The reader should also appreciate the design tension between progressive disclosure (show the user only what they need) and full access (let power users reach everything), and how cc navigates it through deferred loading, search-based discovery, and layered rendering.

Return to the table of contents in the front matter for the full chapter listing and cross-references to HER patterns and failure modes.
