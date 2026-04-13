# Gaps Audit Report for Chapter 16

## Overview vs Synopsis Match

Chapter overview matches the brief synopsis: covers WebFetchTool, WebSearchTool, SSRF guard, web access gating, and prompt-injection threat model. Alignment is good.

## Uncited Brief Source Files

The brief lists 9 source files. Cited files (with line-number citations in code snippets):
- `src/tools/WebFetchTool/WebFetchTool.ts` - cited (3 snippets)
- `src/tools/WebSearchTool/WebSearchTool.ts` - cited (2 snippets)
- `src/utils/hooks/ssrfGuard.ts` - cited (3 snippets)

Uncited brief files:
1. `src/tools/WebFetchTool/utils.ts` - NOT cited with line numbers, but discussed (isPreapprovedUrl, getURLMarkdownContent, applyPromptToMarkdown, checkDomainBlocklist, isPermittedRedirect). Functions are referenced by name but no code snippets.
2. `src/tools/WebFetchTool/preapproved.ts` - NOT cited with line numbers, but discussed (isPreapprovedHost, PREAPPROVED_HOSTS). No code snippets.
3. `src/tools/WebFetchTool/prompt.ts` - NOT cited or discussed.
4. `src/tools/WebFetchTool/UI.tsx` - NOT cited or discussed.
5. `src/tools/WebSearchTool/prompt.ts` - NOT cited or discussed.
6. `src/tools/WebSearchTool/UI.tsx` - NOT cited or discussed.

## Missing Diagrams

Brief requires:
- (a) flowchart of a WebFetch call with SSRF guard - FOUND (flowchart at line 119)
- (b) sequenceDiagram of web-content sanitization feeding into the model - PARTIALLY FOUND. The sequence diagram at line 193 covers the SSRF guard flow, not the "web-content sanitization feeding into the model" topic. The diagram shows DNS lookup and blocked address checking, not the HTML-to-markdown conversion and model processing pipeline.

This is a gap: the brief requires a diagram showing web-content sanitization feeding into the model, but the chapter provides an SSRF guard sequence diagram instead.

## Citation/Diagram/Snippet Counts

- Citation count: 8 (meets minimum of 6)
- Diagram count: 2 (meets minimum of 2)
- Snippet count: 8 (meets minimum of 4)

## Top Files Without Snippets

The top 3 source files most central to the chapter:
1. `src/tools/WebFetchTool/WebFetchTool.ts` - HAS snippets (3)
2. `src/tools/WebSearchTool/WebSearchTool.ts` - HAS snippets (2)
3. `src/utils/hooks/ssrfGuard.ts` - HAS snippets (3)

All top 3 files have snippets. No gap.

## Uncovered Topics

1. **Domain blocklist preflight check** (`checkDomainBlocklist` in utils.ts): The chapter does not mention that WebFetchTool performs a preflight HTTP request to `api.anthropic.com/api/web_fetch/domain_info` to check if a domain is allowed before fetching. This is a significant security layer that the chapter omits entirely.

2. **Redirect handling** (`isPermittedRedirect`, `getWithPermittedRedirects` in utils.ts): The chapter does not discuss the redirect safety check that prevents open-redirect attacks. The `isPermittedRedirect` function only allows same-origin redirects (with www prefix variations). This is a notable defense mechanism.

3. **Content-length and URL-length limits**: The chapter does not mention `MAX_URL_LENGTH` (2000), `MAX_HTTP_CONTENT_LENGTH` (10MB), `FETCH_TIMEOUT_MS` (60s), or `MAX_REDIRECTS` (10) resource consumption controls from utils.ts.

4. **Caching behavior**: The chapter mentions "15-minute cache" in passing but does not discuss the LRU cache implementation, the separate domain-check cache, or the cache-clearing mechanism.
