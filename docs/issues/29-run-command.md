# 29 — `mcplay run` command

## Title
`mcplay run` command (`src/cli/commands/run.ts`)

## Summary
Run a script against a live server, print a report, optionally record a scenario, and exit
with assertion-aware codes.

## Context
The verification/regression surface (DESIGN §6, §10). Composes runner (28), store (13),
config (16), command policy (10).

## Scope
- `mcplay run <script> -- <server-cmd...>` flags: `-o/--out` (also record), `--fail-fast`,
  `--env`, `--cwd`, `--timeout-ms`.
- Load+validate script (Issue 26), connect (Issue 17), run (Issue 28), print report
  (human or `--json`).

## Detailed Requirements
- Exit code `4` on any assertion failure; `0` on all-pass; usage/security codes as defined.
- Target command from argv after `--` (Issue 10).
- If `-o` given, write the recorded scenario via store (Issue 13).

## Acceptance Criteria
- Integration test: passing script exits 0; failing assertion exits 4; `-o` writes a valid
  scenario.
- Test: missing server command → usage error (exit 2).

## Dependencies
- 13, 16, 28 (and 10).

## Validation
- Integration tests with fixture.

## Non-goals
- No mock/replay here. No LLM.

## Design References
- `docs/DESIGN.md` §6, §10.
