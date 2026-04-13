# Cross-Reference Audit: Chapter 22 — Tasks: A Durable Unit of Work

## Required HER References

| HER Ref | Found | Location in Chapter |
|---------|-------|-------------------|
| §10.1 One-Task-Per-Session rule | Yes | ClaimTaskResult section (L104-L122), Edge cases section (L496-L497), Divergence section (L525-L527) |
| §8.2 Three-File State Pattern | Yes | Overview section (L9), Storage layout section (L82), Divergence section (L517-L523) |
| §10.3 Session Protocol | Yes | Overview section (L9) references session protocol context |

## Divergence Section

The "Where cc diverges from the published pattern" section is present and substantive at approximately 285 words. It covers three substantive divergences:

1. **JSON vs Markdown**: cc uses JSON with Zod validation (aligning with the JSON camp) but does not enforce the append-only constraint HER recommends.
2. **No explicit progress file**: cc implements only the structured task list component of HER's three-file pattern, not progress.txt or AGENTS.md.
3. **One-task-per-session not enforced at harness level**: The busy check is opt-in, not mandatory.

Each divergence is substantive and includes specific code references.

## Issues

None. All required HER refs are cited, the divergence section is well over 150 words, and no bad refs were found.

## Verdict: pass
