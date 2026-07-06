# 02 — Error taxonomy & exit-code mapping

## Title
Typed error taxonomy and CLI exit-code mapping

## Summary
Implement `src/core/errors/errors.ts`: a typed error hierarchy and a mapping to the CLI exit
codes defined in `docs/DESIGN.md` §6.

## Context
Consistent, typed errors let every module fail closed with a clear category, and let the CLI
translate failures into stable exit codes (important for CI use of `mcplay run`).

## Scope
- Base `McplayError` with `code` (machine string) and `exitCode` (number).
- Subclasses: `UsageError`(2), `ValidationError`(3), `AssertionError`(4),
  `SecurityPolicyError`(5), `RuntimeError`(1).
- Helper `toExitCode(err): number` and `formatError(err, json?)`.

## Detailed Requirements
- Exit-code map: success 0, generic 1, usage 2, validation/lint 3, assertion 4, security 5
  (exact, per DESIGN §6).
- Errors carry an optional `details` object for `--json` output; never include secret
  values in messages.
- No `process.exit` inside core; core throws, CLI maps (keep core testable).

## Acceptance Criteria
- Unit tests assert each subclass maps to its exit code.
- `formatError` produces both human and JSON forms.

## Validation
- Unit tests in `test/`.

## Dependencies
- 01.

## Non-goals
- No logging concerns (Issue 03).

## Design References
- `docs/DESIGN.md` §6 (exit codes), §14 (failure modes).
