# 30 — `mcplay validate` command

## Title
`mcplay validate` command (`src/cli/commands/validate.ts`)

## Summary
Validate a scenario or script file against its schema and run the security lint, reporting
findings with an appropriate exit code.

## Context
Users and CI need a fast integrity+safety check on files before use/sharing (DESIGN §6).

## Scope
- `mcplay validate <file>` flags: `--strict`, `--json`.
- Detect file type (scenario vs script) by shape/extension; schema-validate (Issue 11/26);
  run lint (Issue 15) for scenarios.
- Print findings; exit `3` on schema failure or (under `--strict`) any warning.

## Detailed Requirements
- Never print detected secret values (report rule + location only).
- `--json` emits machine-readable findings.
- Size/depth limits enforced during load (Issue 09/13).

## Acceptance Criteria
- Test: valid file exits 0; invalid schema exits 3; seeded-secret + `--strict` exits 3.

## Dependencies
- 11, 15, 16, 26.

## Validation
- CLI tests with fixtures.

## Non-goals
- No auto-fix (Issue 34).

## Design References
- `docs/DESIGN.md` §6, §11.
