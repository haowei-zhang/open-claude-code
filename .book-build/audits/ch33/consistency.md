# Consistency Audit Report - Chapter 33

## Term Conflict Check

| Term | Registry Definition | Usage in Chapter | Conflict? |
|------|-------------------|-----------------|-----------|
| classifier | "A function that maps a shell command string to a risk band (read, write, destructive)" | Used broadly for both bash classifier and YOLO classifier, which classifies all tool actions not just shell commands | Mild conflict - registry definition is too narrow for YOLO classifier |
| permission mode | "A named configuration governing tool-access behavior" | Used correctly at line 406 | No |
| feature gate | "A compile-time boundary checked via feature()" | Chapter uses "feature flag" and "GrowthBook feature flag" throughout instead of "feature gate" | Mild - "feature flag" vs "feature gate" |
| side query | "A secondary API call using a smaller model" | Chapter correctly uses `sideQuery` (code name) and describes it as dispatching to a potentially different model | No |
| autocompact | "An automatic compaction trigger" | Used correctly at line 351, 418 | No |
| harness | "The outer runtime layer" | Used correctly at line 9 | No |
| lazySchema | "A wrapper function that defers Zod schema evaluation" | Used correctly at line 72 | No |

## Term Conflicts Found
1. "classifier" - registry defines it narrowly as mapping shell commands to risk bands, but chapter uses it for YOLO classifier which classifies all tool actions. The registry definition should be broadened.
2. "feature flag" vs "feature gate" - the chapter consistently uses "feature flag" (GrowthBook runtime flag) while the registry defines "feature gate" as a compile-time boundary. These are actually different concepts in cc: feature gates are compile-time, feature flags are runtime GrowthBook flags. The chapter is using the correct term for GrowthBook flags.

## Voice Drift
- Chapter is written in present tense, descriptive, cite-heavy style consistent with house style.
- No voice drift detected.

## Proposed New Terms
1. "YOLO classifier" - The LLM-based classifier that evaluates tool actions against a security policy using a two-stage XML pipeline with transcript projection.
2. "transcript projection" - The security-motivated transformation of the raw conversation into a compact transcript that excludes assistant text to prevent classifier manipulation.
3. "two-stage classifier" - A classification pipeline that runs a fast low-token stage first and a slower thinking stage only when the fast stage flags a potential block, reducing latency for clearly safe actions.
