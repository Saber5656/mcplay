# 15 — Scenario security lint

## Title
Scenario security lint (`src/core/scenario/lint.ts`)

## Summary
Beyond schema validation, run security-oriented checks over a loaded scenario/script and
report findings (used by `validate`, `import`, `export`).

## Context
Schema validity is necessary but not sufficient. Shared files may carry unredacted secrets,
suspicious embedded commands, or unusually large payloads (ADR-005 B2/B3).

## Scope
- `lintScenario(scenario): LintReport` with findings of severity `error|warn|info`.
- Checks: `redaction.applied=false` (warn), likely-secret patterns present in payloads
  (warn/error), embedded `target.command` present (info — reminder it will never auto-run),
  oversized/truncated payloads (info), env values improperly present (error).

## Detailed Requirements
- Reuse redaction detectors (Issue 21) for secret-pattern findings.
- `--strict` promotes warnings to errors (exit 3).
- Never print detected secret values; report location + rule only.

## Acceptance Criteria
- Unit test: a scenario with a seeded secret and `redaction.applied=false` yields a warning
  (error under `--strict`).
- Unit test: env-value presence is an error.

## Validation
- Unit tests; consumed by 30, 35, 36.

## Dependencies
- 11, 10 (embedded-command awareness), 21 (detectors — soft dep; may stub then wire).

## Non-goals
- No auto-fix (that is `redact`, Issue 34).

## Design References
- `docs/DESIGN.md` §11.2–§11.5; ADR-005.
