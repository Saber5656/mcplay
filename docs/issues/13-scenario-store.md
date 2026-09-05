# 13 — Scenario store (read/write/atomic/serialize)

## Title
Scenario store: load, validate, atomically write, stable-serialize (`src/core/scenario/store.ts`)

## Summary
Read and write `.mcplay.json` files with schema validation, size/depth limits, atomic writes,
stable serialization, and restrictive file permissions.

## Context
Files are shared and untrusted (ADR-005 B2/B5, ADR-006). Loads must be bounded and
validated; writes must be safe and diff-friendly.

## Scope
- `loadScenario(path): Scenario` — path validated (Issue 08), file-size/JSON-depth limited
  (Issue 09), parsed via schema (Issue 11), migrated if needed (Issue 14).
- `saveScenario(path, scenario)` — atomic write (temp + rename), stable key order + 2-space
  indent, file mode `0600`.
- `listScenarioFiles(dir)` helper for `ls`.

## Detailed Requirements
- Enforce `maxFileBytes` and `jsonDepth` BEFORE full trust.
- Stable serialization so git diffs are meaningful (deterministic key order).
- `0600` mode because a scenario may contain secrets even after redaction attempts.
- Never write partial files on error (atomic rename only after full write).

## Acceptance Criteria
- Unit test: round-trip load(save(x)) === x for a valid scenario.
- Unit test: oversized/deep file rejected with `ValidationError`.
- Unit test: written file has mode 0600 and stable ordering.

## Validation
- Unit tests; used by record/replay/utility commands.

## Dependencies
- 01, 08, 09, 11, 14.

## Non-goals
- No network/remote storage.

## Design References
- `docs/DESIGN.md` §7.1, §11.5–§11.6, §14; ADR-006.
