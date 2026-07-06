# 18 — Transport tap & interaction capture model

## Title
Transport tap and interaction capture (`src/core/recorder/capture.ts`)

## Summary
Observe exact JSON-RPC frames flowing to/from the server, correlate requests with responses
by `id`, capture interleaved notifications, and build `Interaction` records.

## Context
High-level SDK helpers hide raw frames; recording needs exact request/response pairs and
notifications (DESIGN §8.1). This is known-unknown KU1.

## Scope
- Wrap the client transport's `send`/`onmessage` (from Issue 17) to observe frames without
  altering behavior.
- Correlate by JSON-RPC `id`; attach `server->client` notifications observed during a call.
- Produce `Interaction` objects (schema from Issue 11) with `timing` and `truncated` flags,
  applying payload byte caps (Issue 09).

## Detailed Requirements
- If the SDK version disallows clean wrapping, compose around the transport's public
  `send`/`onmessage` (document the chosen approach).
- Apply `capBytes` to large payloads; set `truncated=true`.
- Capture malformed inbound frames as flagged raw entries without crashing (DESIGN §14).
- Do NOT redact here; redaction is a separate pass (Issue 21) so raw capture stays faithful
  until finalize.

## Acceptance Criteria
- Integration test: a `tools/call` yields one `Interaction` with matching request/response.
- Unit test: an oversized payload is truncated with `truncated=true`.
- Unit test: a notification during a call is attached to that interaction.

## Dependencies
- 11, 17 (and 09 for caps).

## Validation
- Integration + unit tests.

## Non-goals
- No matching/replay (Wave 5). No persistence (Issue 13).

## Design References
- `docs/DESIGN.md` §8.1, §14; ISSUE_PLAN KU1.
