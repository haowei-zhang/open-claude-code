# Gaps Audit: Chapter 39

## Brief Synopsis Check

Brief synopsis: "`commands.ts` registry, prompt vs callback types, user-defined commands, argument substitution."

Chapter overview covers:
- commands.ts registry: YES - extensively covered
- prompt vs callback types: YES - three types (prompt, local, local-jsx) covered in detail
- user-defined commands: YES - skill directories, plugin commands covered
- Argument substitution: YES - discussed in control flow section ($ARGUMENTS, named args, etc.)

## Source File Coverage

Brief source files:
1. `src/commands.ts` - CITED extensively (multiple code snippets and references)
2. `src/utils/processUserInput/processSlashCommand.tsx` - DISCUSSED in prose (dispatch path, getMessagesForPromptSlashCommand) but NO code snippet from this file
3. `src/utils/slashCommandParsing.ts` - CITED with code snippet (parseSlashCommand)

Uncited brief files: `src/utils/processUserInput/processSlashCommand.tsx` (discussed but no snippet)

## Mandated Diagrams

Brief requires:
- (a) classDiagram of slash-command types - FOUND (line 299)
- (b) sequenceDiagram of slash-command parsing and dispatch - FOUND (line 388)

All mandated diagrams present.

## Minimum Counts

- Citation count: 10+ (exceeds minimum of 6)
- Diagram count: 3 (exceeds minimum of 2)
- Snippet count: 10 (exceeds minimum of 4)

## Top Files Without Snippets

The top 3 source files by centrality:
1. `src/commands.ts` - HAS snippets (6+ snippets)
2. `src/utils/slashCommandParsing.ts` - HAS snippet (1 snippet)
3. `src/utils/processUserInput/processSlashCommand.tsx` - NO snippet

`processSlashCommand.tsx` is one of the three listed source files and is central to the chapter's topic (the dispatch path), but has no code snippet. The chapter discusses it extensively in prose but never shows actual code from it.

## Uncovered Topics

1. **$ARGUMENTS substitution details** - The chapter mentions argument substitution but does not show the actual substitution code from processSlashCommand.tsx. A reader would expect to see how $ARGUMENTS, $1, $2, etc. are replaced.
2. **Shell command injection (`!` backtick)** - The chapter mentions `executeShellCommandsInPrompt` in passing but does not elaborate on the shell injection mechanism. This is a security-relevant feature that a reader would expect in a chapter about command dispatch.
3. **LocalJSXCommandContext methods** - The chapter describes setMessages, onChangeAPIKey, onChangeDynamicMcpConfig, onInstallIDEExtension but does not show code for how these are wired into the command execution context.

## Summary

- 1 uncited brief file (processSlashCommand.tsx has prose but no snippet)
- 0 missing diagrams
- 1 top file without snippet (processSlashCommand.tsx)
- 3 uncovered topics
- All minimum counts met
