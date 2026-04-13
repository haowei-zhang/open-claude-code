# Consistency Audit: Chapter 51

## Summary
Verdict: **revise** (1 term conflict, no voice drift)

## Term Conflicts
1. "sub-agent" used on line 248; should be "subagent" per terminology registry

## Voice Drift
None detected. Chapter maintains present-tense, descriptive, cite-heavy register consistent with house style.

## Proposed New Terms
1. debug log level — A five-tier verbosity filter (verbose, debug, info, warn, error) controlling which messages are written to the debug log file
2. BufferedWriter — A write abstraction supporting immediate and buffered modes for the debug logging system
3. debug filter pattern — A module/function name pattern via --debug=pattern that filters debug output
4. latest symlink — An advisory symlink at ~/.claude/debug/latest pointing to the current session's debug log
5. immediate mode — The debug writer mode using appendFileSync for crash safety when --debug is active
