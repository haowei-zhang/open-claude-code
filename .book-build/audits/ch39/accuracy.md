# Accuracy Audit: Chapter 39

## Citation Verification

### Snippet: `src/types/command.ts:L175-L207` (CommandBase + Command type)
- File exists: YES
- Line range: L175-206 in actual file (off by 1)
- Issues: The chapter snippet omits `isMcp?: boolean` (line 185 in actual). Minor drift in comment formatting (chapter strips JSDoc comments to inline).
- Verdict: DRIFT (missing field, line range off by 1)

### Snippet: `src/types/command.ts:L168-L174` (CommandAvailability)
- File exists: YES
- Line range: Actual is L169-173 (off by 1)
- Content matches structurally
- Verdict: DRIFT (line numbers off by 1)

### Snippet: `src/commands.ts:L258-L346` (COMMANDS function)
- File exists: YES
- Line range: Correct
- Content matches (truncated with `// ...` markers)
- Verdict: VERBATIM (with documented truncation)

### Snippet: `src/commands.ts:L688-L698` (findCommand)
- File exists: YES
- Line range: Correct
- Content matches
- Verdict: VERBATIM

### Snippet: `src/commands.ts:L449-L469` (loadAllCommands)
- File exists: YES
- Line range: Correct
- Content matches
- Verdict: VERBATIM

### Snippet: `src/commands.ts:L476-L517` (getCommands)
- File exists: YES
- Line range: Correct
- Content matches (slightly reformatted for readability)
- Verdict: VERBATIM

### Snippet: `src/commands.ts:L563-L581` (getSkillToolCommands)
- File exists: YES
- Line range: Correct
- Content matches
- Verdict: VERBATIM

### Snippet: `src/utils/slashCommandParsing.ts:L25-L60` (parseSlashCommand)
- File exists: YES
- Line range: Correct
- Content matches
- Verdict: VERBATIM

### Snippet: `src/commands.ts:L672-L676` (isBridgeSafeCommand)
- File exists: YES
- Line range: Correct
- Content matches
- Verdict: VERBATIM

### Snippet: `src/commands.ts:L547-L559` (getMcpSkillCommands)
- File exists: YES
- Line range: Correct
- Content matches
- Verdict: VERBATIM

## Factual Claims

1. "90+ registered commands" - CONFIRMED: COMMANDS() array contains ~80+ entries plus feature-gated additions
2. "COMMANDS function (memoized)" - CONFIRMED: L258 uses memoize()
3. "Feature-gated commands use feature() Bun bundle macro" - CONFIRMED: code uses feature() checks
4. "INTERNAL_ONLY_COMMANDS gated by USER_TYPE === 'ant'" - CONFIRMED: L343-345
5. "findCommand uses Array.find (first match)" - CONFIRMED: L692
6. "loadAllCommands is memoized by cwd" - CONFIRMED: L449
7. "meetsAvailabilityRequirement not memoized" - CONFIRMED: L417, no memoization wrapper
8. "BRIDGE_SAFE_COMMANDS includes compact, clear, cost, summary, releaseNotes, files" - CONFIRMED: L651-660
9. "REMOTE_SAFE_COMMANDS includes session, exit, clear, help, theme, color, vim, cost, usage, copy, btw, feedback, plan, keybindings, statusline, stickers, mobile" - CONFIRMED: L619-637
10. "parseSlashCommand handles MCP (MCP) suffix" - CONFIRMED: L46-49

## Uncited Sources

- `src/types/command.ts` is extensively cited but NOT listed in the brief's source_files. This is a minor issue as the file is central to the chapter's topic.
- `src/utils/processUserInput/processSlashCommand.tsx` is listed as a source file but not directly cited with line-number snippets (discussed in prose but no fenced code block from it).

## Summary

- 10 snippets total
- 8 verbatim, 2 drift (line number offsets and missing field)
- 0 hallucinated
- 0 unsupported claims
- 2 uncited sources from brief (processSlashCommand.tsx has no code snippet)
