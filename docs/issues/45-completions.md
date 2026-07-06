# 45 — Completions record + mock

## Title
Completions capability — record & mock (`completion/complete`)

## Summary
Support `completion/complete` (argument autocompletion for prompts/resource templates):
capture during recording and reproduce on replay.

## Context
Completions improve UX for prompt/resource-template arguments. Post-MVP, issued separately.

## Scope
- Client op + recorder capture for `completion/complete`.
- Mock responder answering `completion/complete` via the matcher on the completion ref +
  argument.
- Advertise completions capability only if recorded.

## Detailed Requirements
- Match on the canonical completion request (ref + argument name/value).
- Unmatched → `-32001 mcplay/unmatched`.

## Acceptance Criteria
- Integration test: completion request captured and replayed with recorded suggestions.

## Dependencies
- 17, 24.

## Validation
- Integration test.

## Non-goals
- No dynamic/computed completions beyond recordings.

## Design References
- `docs/DESIGN.md` §2.3; research/mcp-protocol-grounding.md.
