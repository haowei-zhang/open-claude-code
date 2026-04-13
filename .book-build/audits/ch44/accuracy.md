# Accuracy Audit: Chapter 44

## Snippet Verification

14 code snippets were verified against source files.

### Verbatim (11)
- `ReplBridgeHandle` (replBridge.ts:L70-L81) - exact match
- `BridgeState` (replBridge.ts:L83) - exact match
- `BackoffConfig` (bridgeMain.ts:L59-L70) - exact match
- `runBridgeLoop` (bridgeMain.ts:L141-L151) - exact match
- `isMultiSessionSpawnEnabled` (bridgeMain.ts:L96-L98) - exact match
- `spawnScriptArgs` (bridgeMain.ts:L119-L124) - exact match
- `sessionWorktrees` (bridgeMain.ts:L177-L184) - exact match
- `sessionIngressTokens` (bridgeMain.ts:L171-L173) - exact match
- `pollSleepDetectionThresholdMs` (bridgeMain.ts:L107-L109) - exact match
- `safeSpawn` (bridgeMain.ts:L126-L139) - exact match
- `crash-recovery pointer` (replBridge.ts:L311-L313) - exact match

### Drift (2)
- `heartbeatActiveWorkItems` (bridgeMain.ts:L202-L270): Strips logForDebugging, logEvent, logger.logVerbose, logger.logError calls and associated try/catch detail.
- `tokenRefresh` (bridgeMain.ts:L279-L313): Strips logger.logVerbose, logger.logError, and logForDebugging calls from the v2 branch.

### Hallucinated (1)
- `BridgeCoreParams` (replBridge.ts:L91-L173): The `onUserMessage` field is shown as `(msg: SDKMessage, done: boolean) => boolean` but the actual source has `(text: string, sessionId: string) => boolean`. The parameter names and types are incorrect. The snippet also omits `perpetual` and `initialSSESequenceNum` fields.

## Factual Claims
- LOC counts are accurate: replBridge.ts is 2406 LOC (chapter says ~2,400), bridgeMain.ts is 2999 LOC (chapter says ~3,000).
- All other factual claims are supported by the source files.
