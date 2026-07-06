# 39 — Resources: replay/mock

## Title
Resources capability — replay/mock responders

## Summary
Extend the mock server to answer `resources/list`, `resources/read`,
`resources/templates/list`, and `resources/subscribe` from a scenario.

## Context
Replay counterpart to Issue 38.

## Scope
- Mock handlers for the resources methods, backed by `catalog.resources` and recorded
  `resources/read` interactions via the matcher (Issue 23).
- Advertise the resources capability in `initialize` only if the scenario recorded it.
- Emit recorded `resources/updated` notifications where present.

## Detailed Requirements
- `resources/read` routed through the matcher on the resource URI (canonicalized).
- Subscribe returns recorded updates deterministically; no live filesystem access.
- Unmatched → `-32001 mcplay/unmatched`.

## Acceptance Criteria
- Integration test: a downstream client lists and reads resources from the mock and receives
  recorded contents.

## Dependencies
- 24, 23.

## Validation
- Integration test.

## Non-goals
- No live resource watching.

## Design References
- `docs/DESIGN.md` §9; §2.3.
