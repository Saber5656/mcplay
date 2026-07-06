# 37 — `mcplay init` command

## Title
`mcplay init` command (`src/cli/commands/init.ts`)

## Summary
Scaffold a config file with documented defaults at the project or global location.

## Context
Lowers first-run friction and documents configurable knobs (DESIGN §13).

## Scope
- `mcplay init` flags: `--global`.
- Write a `mcplay.config.json` populated with the DESIGN §13 defaults and inline comments (as
  a `$schema`/README pointer, since JSON lacks comments — emit a sibling note or JSONC-aware
  doc reference).
- Refuse to overwrite an existing config unless `--force`.

## Detailed Requirements
- Target path validated (Issue 08); project vs global per `--global`.
- Values must match config defaults (Issue 16) exactly.
- Do not overwrite without `--force`.

## Acceptance Criteria
- Test: `init` creates a valid config that `loadConfig` accepts; re-running without `--force`
  refuses.

## Dependencies
- 16.

## Validation
- CLI tests.

## Non-goals
- No interactive wizard in v1.

## Design References
- `docs/DESIGN.md` §13.
