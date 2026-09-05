# 14 — Scenario migration framework

## Title
Scenario `schemaVersion` migration framework (`src/core/scenario/migrate.ts`)

## Summary
A forward-migration pipeline keyed on `schemaVersion` so older scenario files load into the
current schema, and unknown/newer majors are refused.

## Context
ADR-006: the format is a public contract; readers must handle version evolution without
silently mis-parsing.

## Scope
- `migrate(raw: unknown): unknown` applying ordered migrations up to the current version.
- Registry of migration steps `{ from, to, apply }`.
- Refuse unknown/newer major versions with `ValidationError` (exit 3).

## Detailed Requirements
- v1 ships with the identity for `"1"` and a stub registry (no prior versions yet).
- Newer `schemaVersion` than supported → clear, actionable error.
- Migrations are pure functions; no IO.

## Acceptance Criteria
- Unit test: current-version file passes through unchanged.
- Unit test: a fabricated newer version is refused.

## Validation
- Unit tests; used by store (Issue 13).

## Dependencies
- 11.

## Non-goals
- No downgrade migrations.

## Design References
- `docs/DESIGN.md` §14; ADR-006.
