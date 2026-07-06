# 27 — Assertion engine

## Title
Assertion engine (`src/core/script/assert.ts`)

## Summary
Evaluate a step's `expect` clause against a tool-call result and produce pass/fail findings.

## Context
Enables `mcplay run` verification (DESIGN §10) with CI-friendly failures.

## Scope
- `evaluate(expect, result): AssertionOutcome[]`.
- Matchers: `isError` (bool), `resultContains` (substring over serialized text content),
  `resultMatches` (deep-partial JSON-subset), `errorCode` (JSON-RPC error code).
- Uses canonicalization (Issue 12) where structural comparison is needed.

## Detailed Requirements
- Deterministic, side-effect free.
- Clear failure messages including expected vs actual (no secret values printed).
- Multiple expectations per step each yield an outcome.

## Acceptance Criteria
- Unit tests: each matcher passes and fails as designed; `resultMatches` does subset (not
  exact) matching.

## Dependencies
- 11, 12.

## Validation
- Unit tests.

## Non-goals
- No runner/IO (Issue 28). No non-tool assertions in MVP (Issue 46 extends).

## Design References
- `docs/DESIGN.md` §10.
