# 42 — Sampling (server→client) record + mock

## Title
Sampling capability — record & mock (`sampling/createMessage`)

## Summary
Support the server→client `sampling/createMessage` request: capture it during recording (the
client-side response the harness provides) and replay it from the mock's perspective.

## Context
Sampling is a client-provided capability the server calls. This is a bidirectional flow and
is the more complex Wave 8 item (DESIGN §2.3, research note).

## Scope
- Recorder: when a recorded server issues `sampling/createMessage`, capture the request and
  the response the harness returned (v1 harness returns a deterministic canned/no-LLM
  response, since v1 has no LLM — DESIGN §12).
- Mock: when replaying, if the scenario expects the downstream client to satisfy sampling,
  forward/emit the recorded request and match the recorded result.
- Advertise sampling in capabilities only if recorded.

## Detailed Requirements
- v1 has NO LLM: the harness's sampling responder returns a fixed, clearly-marked stub
  response and records it; it must never call any model/provider.
- Bidirectional correlation handled by the capture tap (Issue 18).
- Document the limitation that sampling responses are stubs in v1.

## Acceptance Criteria
- Integration test with a fixture server that requests sampling: the request/response pair is
  captured and reproducible on replay.

## Dependencies
- 17, 18, 24.

## Validation
- Integration test with a sampling-issuing fixture.

## Non-goals
- No real LLM sampling (that is a v2 feature; DESIGN §12).

## Design References
- `docs/DESIGN.md` §2.3, §8.1, §12; research/mcp-protocol-grounding.md.
