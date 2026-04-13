# Consistency Audit: Chapter 34 - Filesystem Permissions and Path Guards

## Terminology Conflicts

Reviewed all terms in terminology.json against chapter usage:

| Term | Used Correctly | Notes |
|---|---|---|
| permission mode | Yes | Used consistently as defined |
| classifier | Yes | Used correctly as "a function that maps shell command string to risk band" - though the chapter extends usage to filesystem classifiers, this is natural |
| harness | Yes | Used correctly |
| fail-closed default | Yes | Used correctly |
| bypass-immune | Yes | Referenced in context of safety checks |
| deny-first evaluation | Yes | Referenced in read check cascade description |
| structural control | Yes | Used correctly when discussing defense-in-depth |
| settings cascade | Yes | Used correctly when discussing session vs user rules |

## Potential Conflicts

1. **"classifier"** - The terminology registry defines classifier as "A function that maps a shell command string to a risk band (read, write, destructive) to drive permission decisions." The chapter uses "classifier" more broadly to include the filesystem permission classifier (auto-mode classifier that auto-approves filesystem operations). This is a mild extension, not a contradiction, since the core concept (mapping input to risk decision) is preserved.

2. **"denial tracking"** vs established terms - The chapter introduces "denial tracking" as a new subsystem term. This is not in the terminology registry and does not conflict with any existing term.

## Voice Drift

No voice drift detected. The chapter is written in present tense, descriptive, cite-heavy style consistent with the house style.

## Proposed New Terms

1. **denial tracking** - "A subsystem that tracks consecutive and total permission denials to prevent classifier death spirals, falling back to interactive prompting after configurable thresholds."
2. **path safety validation** - "The process of checking file paths against dangerous file/directory lists, suspicious Windows patterns, and Claude configuration files before allowing auto-edit operations."
3. **internal path carve-out** - "An exception in the filesystem permission layer that allows the agent to access specific internal paths (plan files, scratchpads, session memory) without explicit permission, even when those paths reside within otherwise dangerous directories."
