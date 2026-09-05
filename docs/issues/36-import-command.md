# 36 — `mcplay import` command

## Title
`mcplay import` command (`src/cli/commands/import.ts`)

## Summary
Import an incoming scenario file: validate schema, run security lint, verify integrity
checksum if present, and place it in the scenarios directory.

## Context
Imported files are untrusted (ADR-005 B2). Import is the entry checkpoint mirroring export.

## Scope
- `mcplay import <file>` flags: `--dir <path>`.
- Steps: size/depth-limited load (09/13), schema-validate (11) + migrate (14), lint (15),
  verify checksum if present, copy into scenarios dir (path-validated, Issue 08).

## Detailed Requirements
- Refuse files failing schema, size/depth caps, or (if present) checksum mismatch (exit 3).
- Surface lint warnings; `--strict` promotes to errors.
- Never auto-execute any embedded command; only stores data.

## Acceptance Criteria
- Test: a valid exported file imports successfully and is listed by `ls`.
- Test: a tampered file (bad checksum) or oversized/deep file is refused.

## Dependencies
- 13, 15 (and 08, 09, 11, 14).

## Validation
- CLI tests including tamper cases.

## Non-goals
- No remote fetch (ADR-004).

## Design References
- `docs/DESIGN.md` §11; ADR-004, ADR-005, ADR-006.
