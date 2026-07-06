# 25 — `mcplay replay` command

## Title
`mcplay replay` command (`src/cli/commands/replay.ts`)

## Summary
Load a scenario and run the mock server on stdio so a downstream MCP client can connect.

## Context
Primary replay surface (DESIGN §6). Composes store (13), config (16), matcher (23), and mock
adapter (24).

## Scope
- `mcplay replay <scenario>` flags: `--match <strict|sequential|best-effort>`,
  `--strict-unmatched`.
- Load + validate scenario (Issue 13), build matcher (Issue 23) with configured/flag mode,
  serve the mock over stdio (Issue 24) until the client disconnects.

## Detailed Requirements
- Default match mode from config (`defaultMatchMode`, default `strict`).
- stdout is exclusively the MCP stream; all human/log output to stderr.
- Clean shutdown on client disconnect / SIGINT.
- Never execute any embedded command (mock is data-only).

## Acceptance Criteria
- Integration test: `replay` a recorded fixture scenario; an SDK client connects and gets
  recorded tool results.
- Test: `--match sequential` changes selection behavior as specified.

## Dependencies
- 13, 16, 24.

## Validation
- Integration test spawning `mcplay replay` and connecting a client.

## Non-goals
- No HTTP endpoint (v2). No recording here.

## Design References
- `docs/DESIGN.md` §6, §9.
