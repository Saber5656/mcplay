# 04 — Test harness + reference MCP server fixture

## Title
Test harness (vitest) and a bundled reference stdio MCP server fixture

## Summary
Set up vitest and create a minimal, deterministic reference MCP server under
`test/fixtures/servers/` used by integration and round-trip tests.

## Context
Round-trip and security tests need a real stdio MCP server to drive. A tiny bundled server
avoids external dependencies and gives deterministic outputs (`docs/DESIGN.md` §15).

## Scope
- `vitest.config.ts`, coverage config (≥80% target on `src/core/**`).
- `test/fixtures/servers/echo-server.mjs`: a stdio MCP server (using the SDK) exposing at
  least tools `echo` (returns its args) and `add` (numeric), with a stable name/version.
- Helper `test/util/spawn-fixture.ts` to launch the fixture for tests.
- A seeded-secret variant tool for redaction tests (returns a fake token in output).

## Detailed Requirements
- The fixture must implement `initialize`, `tools/list`, `tools/call` for the tools above.
- Deterministic outputs (no timestamps/random in tool results).
- Fixture is test-only; not shipped in the npm package `files`.

## Acceptance Criteria
- A smoke test spawns the fixture and lists its tools successfully.
- Coverage reporting runs.

## Validation
- `npm test` runs the smoke test green.

## Dependencies
- 01.

## Non-goals
- No resources/prompts in the MVP fixture (add in Wave 8 issues).

## Design References
- `docs/DESIGN.md` §15 (testing), §4 (SDK usage).
