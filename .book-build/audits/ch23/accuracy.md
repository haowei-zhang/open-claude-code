# Accuracy Audit: Chapter 23 — Teammates and In-Process Collaboration

## Citation Verification

### Verified Citations

1. `src/tools/SendMessageTool/SendMessageTool.ts:L67-L87` — Input schema. **Verified**: Lines 67-87 contain the `inputSchema` with `to`, `summary`, `message` fields. Line numbers are correct.

2. `src/tools/SendMessageTool/SendMessageTool.ts:L46-L65` — StructuredMessage. **Verified**: Lines 46-65 contain the `StructuredMessage` discriminated union with `shutdown_request`, `shutdown_response`, `plan_approval_response`. Correct.

3. `src/tools/SendMessageTool/SendMessageTool.ts:L92-L99` — MessageRouting type. **Verified**: Lines 92-99 contain the `MessageRouting` type. Correct.

4. `src/utils/hooks/sessionHooks.ts:L14-L46` — Function and command hook types. **Verified**: Lines 14-46 contain `FunctionHookCallback`, `FunctionHook`, `SessionHookMatcher`, `SessionStore`. Correct.

5. `src/utils/hooks/sessionHooks.ts:L50-L62` — SessionHooksState. **Verified**: Lines 50-62 contain the `SessionHooksState` type and the Map optimization comment. Correct.

6. `src/tasks/LocalAgentTask/LocalAgentTask.tsx:L162-L167` — queuePendingMessage. **Verified**: Lines 162-167 contain the function. Correct.

7. `src/tasks/LocalAgentTask/LocalAgentTask.tsx:L181-L192` — drainPendingMessages. **Verified**: Lines 181-192 contain the function. Correct.

8. `src/tools/SendMessageTool/SendMessageTool.ts:L802-L873` — Message routing in call(). **Verified**: Lines 802-873 contain the in-process routing logic. Correct.

9. `src/tools/SendMessageTool/SendMessageTool.ts:L195-L266` — handleBroadcast. **Verified**: Lines 191-266 contain the function. Slight off-by-one (function starts at 191, not 195). **Minor drift**.

10. `src/tools/SendMessageTool/SendMessageTool.ts:L305-L399` — handleShutdownApproval. **Verified**: Lines 305-399 contain the function. Correct.

11. `src/tools/SendMessageTool/SendMessageTool.ts:L434-L476` — handlePlanApproval. **Verified**: Lines 434-476 contain the function. Correct.

12. `src/tools/SendMessageTool/SendMessageTool.ts:L585-L601` — checkPermissions. **Verified**: Lines 585-601 contain the function. Correct.

13. `src/tools/SendMessageTool/SendMessageTool.ts:L749-L756` — Re-check bridge handle. **Verified**: Lines 749-756 contain the re-check. Correct.

14. `src/tools/SendMessageTool/SendMessageTool.ts:L678-L696` — Structured message validation. **Verified**: Lines 678-696 contain the validation. Correct.

15. `src/tools/SendMessageTool/SendMessageTool.ts:L667-L676` — validateInput summary required. **Verified**: Lines 667-676 contain the validation. Correct.

16. `src/tools/SendMessageTool/SendMessageTool.ts:L349-L367` — In-process shutdown abort. **Verified**: Lines 348-366 contain the abort logic. Slight off-by-one. **Minor drift**.

17. `src/tools/SendMessageTool/SendMessageTool.ts:L823-L845` — Auto-resume stopped agent. **Verified**: Lines 822-844 contain the auto-resume. Slight off-by-one. **Minor drift**.

18. `src/tools/SendMessageTool/SendMessageTool.ts:L845-L860` — Evicted task handling. **Verified**: Lines 845-860 contain the resume-from-transcript. Correct.

19. `src/utils/hooks/sessionHooks.ts:L64-L108` — addFunctionHook. **Verified**: Lines 64-114 contain the function. Slight range drift (function ends at 114, cited as 108). **Minor drift**.

20. `src/tools/SendMessageTool/SendMessageTool.ts:L142-L150` — agentNameRegistry. **Issue**: No `agentNameRegistry` is defined at those lines. The `agentNameRegistry` is accessed via `appState.agentNameRegistry.get(input.to)` at line 804, but it's not defined in SendMessageTool.ts — it's a property of AppState. The chapter claims it is at L142-L150 in SendMessageTool.ts, but that range is actually the `findTeammateColor` function. **Bad citation**.

## Snippet Verification

1. **Input schema (L67-L87)**: Verbatim match with source.
2. **StructuredMessage (L46-L65)**: Verbatim match with source.
3. **MessageRouting (L92-L99)**: Verbatim match with source.
4. **FunctionHookCallback/FunctionHook/SessionStore (L14-L46)**: Minor drift — chapter omits `OnHookSuccess` type (lines 9-12) and `isHookEqual` import. The `SessionHookMatcher` type in the chapter differs slightly from source (chapter omits `onHookSuccess` in the hooks array items). **Drift**.
5. **SessionHooksState (L50-L62)**: Verbatim match — the Map type with the comment.
6. **queuePendingMessage (L162-L167)**: Verbatim match with source.
7. **drainPendingMessages (L181-L192)**: Verbatim match with source.
8. **addFunctionHook (L64-L108)**: Drift — chapter abbreviates the function body with `// ...adds to session-scoped store`, and the actual function signature differs (source has `sessionId` parameter, `matcher` parameter; chapter omits `sessionId` and `matcher` parameters and shows simplified signature). **Drift**.

## Uncited Source Files

- `src/tools/SendMessageTool/constants.ts` — Only contains `SEND_MESSAGE_TOOL_NAME = 'SendMessage'`. Trivial, informational only.
- `src/tools/SendMessageTool/prompt.ts` — Contains `getPrompt()` function. Not cited.
- `src/tools/SendMessageTool/UI.tsx` — Contains render functions. Not cited.
- `src/utils/hooks/sessionHooks.ts` — Cited extensively.

## Summary

- 1 bad citation (agentNameRegistry location)
- 4 snippet drift issues
- 0 hallucinated snippets
- Total citations: 39 (as reported in manifest)
