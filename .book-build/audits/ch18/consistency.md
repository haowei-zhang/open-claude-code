# Consistency Audit: Chapter 18

## Summary

Verdict: **revise**

## Term Conflicts

No term conflicts found. The chapter uses canonical terms correctly:
- "subagent" (not "sub-agent") - consistent with registry
- "tool" - consistent
- "hook" - consistent
- "permission mode" - consistent
- "fork" - consistent

## Voice Drift

The chapter has a duplicated paragraph: the AgentDefinition introduction appears twice in succession (lines 176 and 178), suggesting a drafting artifact. The takeaways section is also longer than the house style target (~420 words vs 150-300), with some items expanding beyond the typical concise bullet format.

## Proposed New Terms

1. **lazySchema**: A wrapper function that defers Zod schema evaluation until first call, ensuring runtime feature flags are populated before schema construction.
2. **auto-background**: A mechanism that automatically transitions a synchronous subagent to asynchronous execution after a configurable timeout (default 120 seconds).
3. **fork recursion guard**: A dual-guard mechanism preventing infinite recursive agent spawning: a message-scan guard checking FORK_BOILERPLATE_TAG and a querySource guard surviving autocompact.
