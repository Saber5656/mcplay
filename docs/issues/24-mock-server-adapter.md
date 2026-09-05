# 24 — Mock server adapter (tools)

## Title
Mock MCP server adapter (`src/core/replay/server-adapter.ts`)

## Summary
Serve a scenario as an MCP server over stdio using the SDK low-level `Server`, answering
`initialize`, `tools/list`, and `tools/call` from the scenario via the matcher.

## Context
Implements the replay side of ADR-003 (DESIGN §9.1). Uses the low-level `Server` for
fidelity to arbitrary recorded schemas (KU2).

## Scope
- `createMockServer(scenario, matcher): { serve(transport) }`.
- Handlers: `initialize` (return recorded `server` info/capabilities, negotiate protocol
  version), `tools/list` (return `catalog.tools`), `tools/call` (matcher → recorded result
  or error).
- Emit recorded `notifications` verbatim where applicable.

## Detailed Requirements
- Use `StdioServerTransport`; stdout carries protocol only (logs to stderr, Issue 03).
- Never read files or execute the embedded `target.command` — data only (ADR-005 B4).
- Unmatched request → JSON-RPC error from the matcher (`-32001 mcplay/unmatched`).
- Reproduce recorded values verbatim; do not synthesize nondeterministic fields (DESIGN §9.3).

## Acceptance Criteria
- Integration test: a downstream SDK `Client` connects to the mock, lists tools, and calls a
  tool receiving the recorded result.
- Test: unmatched call returns the defined error.
- Test: mock never touches the filesystem for tool results.

## Dependencies
- 11, 13, 23.

## Validation
- Integration test with an in-process SDK client.

## Non-goals
- No resources/prompts responders (Issues 39, 41). No HTTP transport.

## Design References
- `docs/DESIGN.md` §9.1, §9.3, §11.1 (B4); ADR-003; ISSUE_PLAN KU2.
