# 28 — Script runner

## Title
Script runner (`src/core/script/runner.ts`)

## Summary
Execute a parsed script's steps sequentially against a live server via the client harness,
evaluating assertions and (optionally) recording.

## Context
Shared by `mcplay run` (Issue 29) and non-interactive `mcplay record --script` (Issue 22).
Reuses recorder harness (17/19) so a run can also produce a scenario.

## Scope
- `runScript(script, session, { assert, record }): RunReport`.
- For each step: perform the action via the client (17), capture (18) if recording, evaluate
  `expect` via Issue 27, accumulate outcomes.
- `--fail-fast` stops at first failing assertion; otherwise run all and aggregate.

## Detailed Requirements
- Deterministic ordering; per-request timeout from limits.
- `RunReport` includes per-step outcomes and an overall pass/fail.
- When recording, produce a schema-valid `Scenario` alongside the report.

## Acceptance Criteria
- Integration test against fixture: a passing script → all-pass report; a wrong `expect` →
  failing report; `--fail-fast` stops early.
- Test: `record:true` yields a valid scenario.

## Dependencies
- 17, 19, 26, 27.

## Validation
- Integration tests with fixture.

## Non-goals
- No CLI/exit-code mapping (Issue 29). No resources/prompts steps (Issue 46).

## Design References
- `docs/DESIGN.md` §10, §8.
