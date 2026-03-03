# Changelog

## 0.4.0 (2026-03-04)

### Features

- **`readOnlyHint` on `DiscoveredTool`**: Added optional `readOnlyHint?: boolean` field to the `DiscoveredTool` interface, matching the spec's formalized tool definition struct where `readOnlyHint` is promoted from annotations to a top-level field
- **`ToolRegisterErrorEvent`**: New event type emitted when `registerTool()` throws `InvalidStateError` (duplicate tool name or empty name/description)
- **`ToolUnregisterErrorEvent`**: New event type emitted when `unregisterTool()` throws `InvalidStateError` (tool name not found)
- **`prepare` script**: Added `tsup` as `prepare` script so the package auto-builds when installed from GitHub URLs

### Spec alignment

- `DiscoveredTool` JSDoc updated to reference the spec's formalized "tool definition" struct with five fields: name, description, input schema (DOMString), execute steps, and read-only hint (boolean, initially false)
- Error event types align with the spec's formalized `registerTool()` and `unregisterTool()` algorithms which now throw `InvalidStateError` on validation failures

## 0.2.0 (2026-02-17)

### Features

- **`ToolCallResultContent` type**: New union type for rich tool call results (`text` and `image` content blocks), mirroring MCP's `TextContent` and `ImageContent` types

### Breaking changes

- **`ToolSource.callTool()` return type**: Changed from `Promise<string | null>` to `Promise<ToolCallResultContent[]>` to support rich responses (text, images) from tool sources like Playwright browser automation
- Updated `ToolSource` interface docs to list `PlaywrightBrowserSource` as a new implementation

## 0.1.0 (2026-02-17)

### Features

- **Tool types**: `WebMCPTool`, `DiscoveredTool`, `ToolAnnotations` aligned with WebMCP WebIDL spec
- **Structured content types**: `ToolContentText`, `ToolContentJSON`, `ToolContent` matching react-webmcp library
- **JSON Schema types**: `JSONSchema`, `JSONSchemaProperty` for tool input/output schemas
- **ToolSource interface**: `ToolSource`, `ToolSourceConfig` abstraction for CDP and extension sources
- **Event types**: `InspectorEvent` discriminated union covering all intercepted events:
  - Tool lifecycle: `TOOL_REGISTERED`, `TOOL_UNREGISTERED`, `CONTEXT_CLEARED`
  - AI sessions: `SESSION_CREATED`, `PROMPT_SENT`, `PROMPT_RESPONSE`, `PROMPT_ERROR`
  - Streaming: `STREAM_START`, `STREAM_END`
  - Tool execution: `TOOL_CALL`, `TOOL_RESULT_AI`, `TOOL_ACTIVATED`, `TOOL_CANCEL`
  - Navigation: `PAGE_RELOAD`
- **Protocol messages**: Full type definitions for all communication channels:
  - `ExtToServerMessage` / `ServerToExtMessage` (extension <-> server WebSocket)
  - `ContentToBackgroundMessage` (content script -> background SW)
  - `BackgroundToPanelMessage` / `PanelToBackgroundMessage` (background <-> DevTools panel)

### Spec alignment

- `WebMCPTool.inputSchema` typed as `JSONSchema | Record<string, never>` (object, not string) per WebIDL `ToolRegistrationParams`
- `DiscoveredTool.inputSchema` typed as `string` (DOMString) per WebIDL `RegisteredTool` from `modelContextTesting.listTools()`
- `ToolAnnotations.readOnlyHint` is the only browser-native annotation; `destructiveHint`, `idempotentHint`, `cache` are library extensions
- Optional `outputSchema` on `WebMCPTool` (library extension, ignored by browser)
