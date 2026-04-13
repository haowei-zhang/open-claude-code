# Accuracy Audit - Chapter 14: The Bash Tool, Classifiers, and Sandboxing

## Citation Verification

### Verified Citations
- `src/utils/permissions/bashClassifier.ts` - ClassifierResult and ClassifierBehavior types verified at lines 5-12 (chapter says L6-L13, off by 1)
- `src/tools/BashTool/commandSemantics.ts:L10-L17` - CommandSemantic type verified, exact match
- `src/tools/BashTool/shouldUseSandbox.ts` - SandboxInput type verified at lines 13-16 (chapter says L3-L6, wrong lines)
- `src/tools/BashTool/bashSecurity.ts:L77-L101` - BASH_SECURITY_CHECK_IDS verified, exact match
- `src/tools/BashTool/bashSecurity.ts:L16-L41` - COMMAND_SUBSTITUTION_PATTERNS verified
- `src/tools/BashTool/bashSecurity.ts:L45-L74` - ZSH_DANGEROUS_COMMANDS verified
- `src/tools/BashTool/bashSecurity.ts:L128-L174` - extractQuotedContent verified
- `src/tools/BashTool/shouldUseSandbox.ts:L130-L153` - shouldUseSandbox function verified
- `src/tools/BashTool/shouldUseSandbox.ts:L82-L101` - Fixed-point iteration verified
- `src/tools/BashTool/destructiveCommandWarning.ts:L12-L17` - DESTRUCTIVE_PATTERNS start verified
- `src/tools/BashTool/modeValidation.ts:L7-L15` - ACCEPT_EDITS_ALLOWED_COMMANDS verified
- `src/tools/BashTool/bashSecurity.ts:L103-L117` - ValidationContext type verified

### Issues
1. **Line number drift** - `bashClassifier.ts` snippet cites L6-L13 but actual content is at L5-L12
2. **Line number drift** - `shouldUseSandbox.ts` SandboxInput snippet cites L3-L6 but actual content is at L13-L16
3. **Missing caption** - First code block is a comment placeholder, not a real snippet with source-path caption
4. **Composite snippet** - `readOnlyValidation.ts:L128-L150` merges CommandConfig type (line 35) with COMMAND_ALLOWLIST (line 128+)

### Snippet Verification
- Total snippets: 18
- Verbatim: 12
- Drift (line numbers off or minor whitespace): 5
- Hallucinated: 0
- Missing proper caption: 1

### Uncited Sources
The chapter does not cite:
- `src/tools/BashTool/bashPermissions.ts`
- `src/tools/BashTool/pathValidation.ts` (mentioned in text but no snippet)
- `src/tools/PowerShellTool/*` (all 12 files)

Note: PowerShellTool files are informational only - the chapter focuses on Bash.

### Factual Accuracy
- The claim about `COMMAND_ALLOWLIST` being "nearly 2000 lines" is accurate (readOnlyValidation.ts is ~1991 lines)
- The description of the classifier as "a stub for external builds" is accurate per bashClassifier.ts header comment
- The description of `dangerouslyDisableSandbox` naming convention is accurate
- The description of `containsExcludedCommand` splitting compound commands is accurate
