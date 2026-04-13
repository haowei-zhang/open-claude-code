# Gaps Audit: Chapter 36 - The Hook Schema and Lifecycle Events

## Brief Synopsis Match
- Brief: "Hook schema in `schemas/hooks.ts`. Each event documented with trigger, payload, effect. Hook types: command, prompt, http, agent, function. Exit code semantics."
- Chapter covers all of these topics. Overview matches synopsis. PASS.

## Source File Citation Check

| Source File | Cited? |
|-------------|--------|
| src/schemas/hooks.ts | YES (extensively) |
| src/utils/hooks.ts | NO - **GAP** |
| src/utils/hooks/hooksSettings.ts | YES (extensively) |
| src/utils/hooks/hooksConfigManager.ts | YES (extensively) |

`src/utils/hooks.ts` is a 5022 LOC file that implements the actual hook execution runtime (spawning commands, running prompt hooks, HTTP hooks, agent hooks, managing timeouts, exit code processing). This is the most significant source file in the brief and is completely uncited. The chapter covers the schema and configuration but not the execution pipeline.

## Mandated Diagrams

| Required | Found? |
|----------|--------|
| (a) classDiagram of hook schema | YES |
| (b) erDiagram of all events with triggers | NO - **MISSING** |
| (c) flowchart of exit-code handling | NO - **MISSING** |

## Minimum Counts

| Metric | Required | Actual | Pass? |
|--------|----------|--------|-------|
| Citations | 6 | 20 | YES |
| Diagrams | 2 | 2 | YES (bare minimum) |
| Snippets | 4 | 20 | YES |

## Top 3 Source Files Without Snippets

1. `src/utils/hooks.ts` - 5022 LOC, the main hook execution runtime. NO snippets. **GAP** - this is the most important file and has zero coverage.
2. `src/schemas/hooks.ts` - Has snippets. OK.
3. `src/utils/hooks/hooksConfigManager.ts` - Has snippets. OK.

## Uncovered Topics

1. **Hook execution runtime**: The chapter covers the schema and configuration but does not discuss how hooks are actually executed (command spawning, process management, timeout enforcement, error handling in `src/utils/hooks.ts`). This is the single biggest gap.
2. **Hook result processing**: How exit codes are interpreted at runtime, how stderr/stdout are captured and routed to model vs user.
3. **Hook timeout enforcement**: The timeout field in the schema is documented, but how timeouts are enforced (process killing, SIGTERM/SIGKILL sequence) is not covered.
4. **Hook input serialization**: How hook input JSON is constructed and passed to each hook type (stdin, command-line args, HTTP body).
