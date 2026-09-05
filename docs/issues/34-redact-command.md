# 34 — `mcplay redact` command

## Title
`mcplay redact` command (`src/cli/commands/redact.ts`)

## Summary
Apply redaction rules to an existing scenario, writing a redacted copy and updating the
`redaction` metadata.

## Context
Users may need to redact a scenario recorded with `--no-redact`, or re-run stricter rules
before sharing (DESIGN §11.5).

## Scope
- `mcplay redact <scenario>` flags: `-o/--out`, `--rule <name>` (repeatable).
- Load (Issue 13), apply redaction (Issue 21), write result (Issue 13) with updated
  `redaction.applied=true` and `rules`.

## Detailed Requirements
- Default rules = config redaction rules; `--rule` overrides/augments.
- Never print secret values; report a count of redactions by rule.
- Output defaults to writing in place with a confirmation, or to `--out`.

## Acceptance Criteria
- Test: redacting a seeded-secret scenario removes the secret and marks it redacted.

## Dependencies
- 13, 21.

## Validation
- CLI tests.

## Non-goals
- No detection reporting beyond counts (that is `validate`).

## Design References
- `docs/DESIGN.md` §11.5.
