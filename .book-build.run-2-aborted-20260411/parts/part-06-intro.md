# Part 6: Safety, Permissions, and Hooks

An agent that can execute arbitrary shell commands, write any file, and call any API without restriction is not an assistant -- it is a loaded weapon pointed at the user's system. The model does not know that `rm -rf /` is catastrophic; it only knows that the pattern matches a request it received. Part 6 addresses the defense-in-depth architecture that stands between the model's raw capabilities and the user's actual intent: permission modes, command classification, filesystem guards, the gatekeeping hook that mediates every tool call, and the lifecycle hooks that let external processes observe and intervene.

This part groups six chapters around a single theme: the guardrail stack. The chapters proceed from policy declaration down through enforcement, from the permission model that defines what is allowed, through the classifiers that assess risk, through the filesystem guards that prevent path traversal, through the engine hook that renders every decision, and into the hook system that makes the guardrail extensible.

Chapter 32, "The Permission Model: Modes and Rules," defines the six permission modes (default, plan, acceptEdits, bypassPermissions, dontAsk, auto) and the rule parsing and evaluation engine that determines whether a given tool invocation is allowed, denied, or requires user confirmation.

Chapter 33, "Bash Classifier and YOLO Scoring," opens bashClassifier.ts and the YOLO classifier to show how shell commands are mapped to risk bands, how the classifier pipeline works, and how the auto permission mode uses classification scores to decide autonomously.

Chapter 34, "Filesystem Permissions and Path Guards," covers the filesystem permission module: symlink traversal defense, glob allow/deny patterns, and the denial tracking system that records rejected file accesses so the model can adjust its behavior.

Chapter 35, "`useCanUseTool`: The Engine Hook," descends into the approximately 40,000-line React hook that stands at the center of the permission system. This chapter traces how a tool request arrives, how the hook evaluates it against rules and mode, how it interacts with the approval UI, and how plan mode enforcement gates write operations.

Chapter 36, "The Hook Schema and Lifecycle Events," shifts from the internal permission system to the external hook system: the hook schema in schemas/hooks.ts, each lifecycle event with its trigger, payload, and effect, the five hook types (command, prompt, http, agent, function), and the exit code semantics that allow hooks to block, modify, or permit tool execution.

Chapter 37, "Hook Execution: Command, Prompt, HTTP, Agent, Function," completes the picture with the execution pipelines for each hook type, the async hook registry, the SSRF guard that prevents hooks from reaching internal services, and the session hooks that bind external processes into the agent's lifecycle.

These chapters belong together because they form a single enforcement stack. The permission model declares policy; the classifiers assess risk; the filesystem guards prevent traversal; the engine hook renders decisions; the hook schema makes the system extensible; and the execution pipelines bring hooks to life. Each layer depends on the ones below it, and together they implement the principle that no tool invocation reaches the outside world without passing through at least one gate that can say no.

By the end of this part, the reader should understand how cc constructs its defense-in-depth: that permission is not a single Boolean but a multi-layered decision involving mode, rules, classifiers, and user confirmation. The reader should also grasp why the hook system exists alongside the permission system -- the former is an internal guardrail, the latter is an extensibility point -- and why separating them matters for both security and composability.

Return to the table of contents in the front matter for the full chapter listing and cross-references to HER patterns and failure modes.
