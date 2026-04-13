# Gaps Audit Report - Chapter 25: Messages and the Conversation Model

## Overview vs Synopsis Match

The chapter's Overview section accurately reflects the brief's synopsis: "`src/utils/messages.ts` and `src/utils/attachments.ts` -- message types, `<history_snip>`, compact boundaries, attachments, content replacement." The Overview covers message types, compaction boundaries, attachments, and the message pipeline. However, the synopsis mentions `<history_snip>` and "content replacement" explicitly, and while the chapter references `<history_snip>` briefly (in the short message IDs section), it does not have a dedicated section explaining the history_snip mechanism. Content replacement is not explicitly covered.

## Source File Citation

Brief source files:
1. `src/utils/messages.ts` - CITED (19 code snippets + numerous prose references)
2. `src/utils/attachments.ts` - CITED (3 code snippets + prose references)

All source files cited. PASS.

## Mandated Diagrams

Brief requires:
- (a) classDiagram of message types - NOT FOUND. Both diagrams are flowcharts.
- (b) flowchart of message lifecycle - FOUND (Diagram 1)

Missing mandated diagram: classDiagram of message types.

## Minimum Counts

- Citation count: 19 (minimum 6) - PASS
- Diagram count: 2 (minimum 2) - PASS
- Snippet count: 19 (minimum 4) - PASS

## Top Files Without Snippets

The top 2 source files are `src/utils/messages.ts` and `src/utils/attachments.ts`. Both have snippets. PASS.

## Uncovered Topics

1. **`<history_snip>` mechanism**: The synopsis explicitly mentions `<history_snip>` but the chapter only references it in passing (in the "Short message IDs for snip referencing" subsection). The actual snipping mechanism -- how messages are tagged with `[id:...]` markers and then trimmed by the snip projection system -- is not explained. This is a gap given the synopsis explicitly lists it.

2. **Content replacement**: The synopsis mentions "content replacement" but the chapter does not explicitly discuss how message content is replaced during compaction or how the content replacement pipeline works (e.g., replacing tool results with summaries).

3. **`reorderMessagesInUI` function**: Mentioned twice (with a duplicate paragraph) but never shown in code. This is a key UI-layer function that the chapter describes but does not substantiate with a snippet.

4. **`normalizeMessagesForAPI` function**: Described in detail in the "API normalization and message filtering" subsection (6 transformations listed) but no code snippet is shown for this critical function. This is the most important function in the chapter's topic and it lacks a snippet.
