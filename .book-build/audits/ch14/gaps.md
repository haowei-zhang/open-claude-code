# Gaps Audit - Chapter 14: The Bash Tool, Classifiers, and Sandboxing

## Overview vs Synopsis Match
- Synopsis: "BashTool and its safety subsystem. Command classification, destructive patterns, mode validation, sandbox detection, PowerShell variant."
- Chapter covers: BashTool, safety subsystem, command classification, destructive patterns, mode validation, sandbox detection
- GAP: PowerShell variant not covered in the chapter

## Uncited Brief Files
Source files listed in the brief not cited in the chapter:
1. `src/tools/BashTool/bashPermissions.ts` - NOT cited
2. `src/tools/BashTool/pathValidation.ts` - Referenced in text but no snippet
3. `src/tools/BashTool/prompt.ts` - NOT cited
4. `src/tools/PowerShellTool/PowerShellTool.tsx` - NOT cited (PowerShell variant not covered)
5. `src/tools/PowerShellTool/powershellSecurity.ts` - NOT cited
6. `src/tools/PowerShellTool/powershellPermissions.ts` - NOT cited
7. `src/tools/PowerShellTool/commandSemantics.ts` - NOT cited
8. `src/tools/PowerShellTool/destructiveCommandWarning.ts` - NOT cited
9. `src/tools/PowerShellTool/modeValidation.ts` - NOT cited
10. `src/tools/PowerShellTool/pathValidation.ts` - NOT cited
11. `src/tools/PowerShellTool/readOnlyValidation.ts` - NOT cited
12. `src/tools/PowerShellTool/prompt.ts` - NOT cited
13. `src/tools/PowerShellTool/toolName.ts` - NOT cited
14. `src/utils/permissions/bashClassifier.ts` - CITED

Total uncited brief files: 12 (excluding bashClassifier.ts which IS cited)
Note: 11 of these are PowerShellTool files. The chapter explicitly does not cover the PowerShell variant.

## Missing Diagrams
1. Required: "classDiagram of the classifier pipeline" - NOT FOUND
   - The chapter has a flowchart and a stateDiagram-v2 but no classDiagram

## Minimum Counts
- Citation count: 15 (min: 6) - MET
- Diagram count: 2 (min: 2) - MET
- Snippet count: 18 (min: 4) - MET

## Top Source Files Without Snippets
The top 3 source files most central to the chapter topic:
1. `src/tools/BashTool/BashTool.tsx` - No snippet (only a comment placeholder)
2. `src/tools/BashTool/bashSecurity.ts` - HAS snippets (3 snippets)
3. `src/tools/BashTool/readOnlyValidation.ts` - HAS snippets (3 snippets)

Top file without snippet: BashTool.tsx (only has a comment placeholder, not a real code snippet)

## Uncovered Topics
1. PowerShell variant - Explicitly mentioned in the synopsis but not covered in the chapter
2. bashPermissions.ts - The permission rule system is referenced but not detailed
3. The BashTool.tsx main component - Its structure and how it integrates the subsystems is not shown

## Summary
- 12 uncited brief files (11 are PowerShellTool)
- 1 missing mandated diagram (classDiagram)
- 1 top file without real snippet (BashTool.tsx)
- 2-3 uncovered topics
