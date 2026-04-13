# Gaps Audit Report - Chapter 33

## Brief Summary
- Title: "Bash Classifier and YOLO Scoring"
- Synopsis: "bashClassifier.ts, yoloClassifier, classifier approvals. How commands are mapped to risk bands and how auto mode uses classification."
- Source files: ["src/utils/permissions/bashClassifier.ts"]
- Required diagrams: (a) flowchart of the classifier decision tree, (b) classDiagram of classifier inputs
- HER refs: Section 5 Pattern 10, Section 12.4

## Check 1: Overview matches synopsis
- Overview covers both Bash classifier and YOLO classifier, explaining risk classification and auto mode usage.
- VERIFIED.

## Check 2: Source file citation
- Brief lists only `src/utils/permissions/bashClassifier.ts`
- The chapter extensively cites `bashClassifier.ts` (4 snippets) - CITED.
- The chapter also heavily references `yoloClassifier.ts` (11 snippets), which is NOT in the brief's source_files list but is clearly the primary subject matter given the synopsis mentions "yoloClassifier".
- This is a brief completeness issue, not a chapter gap.

## Check 3: Required diagrams
- (a) flowchart of the classifier decision tree: Present as "Transcript Assembly Flowchart" - partially matches. The flowchart covers transcript assembly and format selection, not the full classification decision tree from input to block/allow outcome.
- (b) classDiagram of classifier inputs: NOT PRESENT. No classDiagram in the chapter.
- MISSING DIAGRAM: classDiagram of classifier inputs.

## Check 4: Minimum counts
- Citation count: 25 (>= 6) VERIFIED
- Diagram count: 2 (>= 2) VERIFIED
- Snippet count: 15 (>= 4) VERIFIED

## Check 5: Top 3 source files with snippets
- Top source: bashClassifier.ts (only file in brief) - 4 snippets PRESENT
- The chapter also covers yoloClassifier.ts extensively with 11 snippets
- VERIFIED.

## Check 6: Missing topics
- "YOLO scoring" is in the chapter title but the chapter focuses on the YOLO classifier's architecture rather than the scoring mechanism itself. The "yolo" name and its origin/meaning are not explained.
- "Classifier approvals" mentioned in the synopsis but not explicitly discussed as a separate topic. The chapter covers classification results but doesn't have a dedicated section on the approval/denial flow from the user's perspective.
- The brief says "How commands are mapped to risk bands" - the chapter covers transcript projection and model-based classification but doesn't show explicit risk-band mapping (e.g., read/write/destructive categories).

## Summary
- uncited_brief_files: none
- missing_diagrams: 1 (classDiagram of classifier inputs)
- uncovered_topics: ["YOLO scoring name origin/meaning", "classifier approvals from user perspective", "explicit risk-band mapping"]
- citation_count: 25
- diagram_count: 2
- snippet_count: 15
- top_files_without_snippets: none
