# 17 — MCP client wrapper (connect / initialize / tools)

## Title
MCP client wrapper (`src/core/mcp/client.ts`)

## Summary
Wrap `@modelcontextprotocol/sdk` `Client` + `StdioClientTransport` to connect to a spawned
server, perform `initialize`/capability negotiation, and expose tools `list`/`call`.

## Context
The recorder and script runner drive a real server through this wrapper (DESIGN §8). It must
use the secure spawner (Issue 06), not spawn directly.

## Scope
- `connect({ command, args, env, cwd, limits }): McpClientSession` that spawns via Issue 06
  and connects the SDK `Client` over the spawned stdio.
- Expose `initialize()` result (server info, capabilities, negotiated protocol version),
  `listTools()`, `callTool(name, args)`, and `close()`.
- Surface the underlying transport for the capture tap (Issue 18).

## Detailed Requirements
- Target protocol `2025-11-25`; store the negotiated version returned by the server.
- Per-request timeout via Issue 09.
- Clean shutdown always disposes the child (Issue 06).
- Do not swallow protocol errors; propagate as typed errors.

## Acceptance Criteria
- Integration test against the fixture server (Issue 04): connect, initialize, list `echo`
  and `add`, call `add` → correct result.
- Negotiated protocol version is captured.

## Dependencies
- 06, 07, 11.

## Validation
- Integration test with fixture; unit tests for error propagation.

## Non-goals
- No resources/prompts here (Wave 8: Issues 38, 40). No HTTP transport.

## Design References
- `docs/DESIGN.md` §4, §8.1; research/mcp-protocol-grounding.md.
