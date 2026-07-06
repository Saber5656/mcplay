# 48 — Round-trip integration test suite

## Title
Record → replay → re-call round-trip integration tests

## Summary
Prove the core value: recording a fixture server, replaying it as a mock, and re-issuing the
same calls yields identical results (golden equality).

## Context
This is the end-to-end correctness gate for the MVP (DESIGN §15 "Round-trip property").

## Scope
- Record the fixture server's `tools/list` + `tools/call` (echo/add) into a scenario.
- Launch `mcplay replay` on that scenario.
- Connect a downstream SDK client to the mock and re-issue the same calls.
- Assert results equal the originally recorded results (golden comparison).
- Cover all three match modes' expected behaviors on a representative sequence.

## Detailed Requirements
- Deterministic (fixture outputs are stable).
- Include a negative case: an unrecorded call yields `-32001 mcplay/unmatched` in strict mode.
- Run in CI (Node 20 + 22).

## Acceptance Criteria
- Round-trip golden test passes; negative case behaves as specified.

## Dependencies
- 22, 25, 29, 04.

## Validation
- CI integration suite.

## Non-goals
- Capability round-trips (resources/prompts/etc.) are covered by their own Wave 8 issue
  tests.

## Design References
- `docs/DESIGN.md` §15, §9.
