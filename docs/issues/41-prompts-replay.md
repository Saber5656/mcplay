# 41 — Prompts: replay/mock

## Title
Prompts capability — replay/mock responders

## Summary
Extend the mock server to answer `prompts/list` and `prompts/get` from a scenario.

## Context
Replay counterpart to Issue 40.

## Scope
- Mock handlers for prompts methods backed by `catalog.prompts` and recorded `prompts/get`
  interactions via the matcher.
- Advertise prompts capability in `initialize` only if recorded.

## Detailed Requirements
- `prompts/get` routed through the matcher on name+arguments (canonicalized).
- Unmatched → `-32001 mcplay/unmatched`.

## Acceptance Criteria
- Integration test: a downstream client lists and gets prompts from the mock with recorded
  content.

## Dependencies
- 24, 23.

## Validation
- Integration test.

## Non-goals
- No dynamic prompt templating beyond recorded results.

## Design References
- `docs/DESIGN.md` §9, §2.3.
