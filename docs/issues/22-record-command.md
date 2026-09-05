# 22 — `mcplay record` command + REPL

## Title
`mcplay record` command and interactive REPL (`src/cli/commands/record.ts`, `src/cli/repl.ts`)

## Summary
Wire the recorder into a CLI command with an interactive REPL for driving tools and a
non-interactive scripted mode, writing a scenario file at the end.

## Context
This is the primary record surface (DESIGN §6, §8.3). It composes recorder core (19),
store (13), config (16), and command policy (10).

## Scope
- `mcplay record -- <server-cmd...>` with flags: `-o/--out`, `--script`, `--env`, `--cwd`,
  `--name`, `--no-redact`, `--timeout-ms`, `--max-bytes`.
- REPL commands: `tools|list`, `call <tool> <json-args>`, `raw <method> <json-params>`,
  `redact`, `save [file]`, `quit`, `help`.
- Non-interactive: if `--script` given or stdin not a TTY, feed the recorder from a script
  (Issue 28 runner) instead of the REPL.

## Detailed Requirements
- The target command comes from argv after `--` (Issue 10 `assertCliCommandAllowed`).
- On `save`, write via store (Issue 13) to `--out` or `<scenariosDir>/<name>.mcplay.json`.
- Logs go to stderr only (Issue 03) so any piped output stays clean.
- Exit codes per DESIGN §6; security refusals exit 5.

## Acceptance Criteria
- Integration test: scripted record against fixture writes a schema-valid scenario file.
- Test: REPL `call add {"a":1,"b":2}` captures the interaction.
- Test: no target command after `--` → usage error (exit 2).

## Dependencies
- 13, 16, 19, 21, 10.

## Validation
- Integration tests (scripted mode is deterministic).

## Non-goals
- No resources/prompts REPL verbs (Wave 8). No live re-drive/asserts (that is `run`).

## Design References
- `docs/DESIGN.md` §6, §8.3.
