# 21 — Redaction engine (rules + redactor)

## Title
Redaction engine (`src/core/redaction/rules.ts`, `redactor.ts`)

## Summary
Detect and redact likely secrets in captured payloads and structurally ensure env values are
never persisted; on by default during recording.

## Context
ADR-005 §11.5: recorded data may contain secrets that must be removed before sharing.
Redaction detectors are also reused by the security lint (Issue 15).

## Scope
- Rules: `default-secrets` (Authorization values, `*_token`/`*_secret`/`*_key`/`password`/
  `apiKey` fields, AWS/GCP key shapes, high-entropy strings), `env-values` (structural).
- `redactScenario(scenario, rules): { scenario, appliedRules }` returning a redacted copy.
- `detectSecrets(value): Finding[]` (shared with lint), reporting location + rule, never the
  value.

## Detailed Requirements
- Replace redacted values with a stable placeholder (e.g. `"<redacted:default-secrets>"`).
- `env-values` is structural: env values are never present to begin with (Issue 07); the
  rule asserts/enforces this.
- On by default; `--no-redact` sets `redaction.applied=false` and emits a warning.
- Deterministic output (stable placeholders) for diff-friendliness.

## Acceptance Criteria
- Unit tests: seeded token in a tool result is replaced; field-name and high-entropy rules
  fire; no secret value appears in `Finding`s.
- Test: `redaction.applied`/`rules` recorded accurately.

## Dependencies
- 11.

## Validation
- Unit tests; reused by 15, 34, 35.

## Non-goals
- No unredaction. Tuning against real servers is KU4 (may add rules later).

## Design References
- `docs/DESIGN.md` §11.5; ADR-005; ISSUE_PLAN KU4.
