# 44 — Elicitation (server→client) record + mock

## Title
Elicitation capability — record & mock (`elicitation/create`)

## Summary
Support the server→client `elicitation/create` request: capture the elicitation prompt and
the (scripted/stub) client response, and reproduce on replay.

## Context
Elicitation lets a server ask the client for structured input. Bidirectional, similar to
sampling (DESIGN §2.3).

## Scope
- Recorder: capture `elicitation/create` requests and the harness's returned response. In
  interactive record the user may answer; in scripted/non-interactive mode a scripted/stub
  answer is used and recorded.
- Mock: reproduce the recorded elicitation exchange deterministically.
- Advertise elicitation only if recorded.

## Detailed Requirements
- No secret leakage: elicited values are subject to redaction (Issue 21).
- Bidirectional correlation via the capture tap (Issue 18).
- Deterministic replay.

## Acceptance Criteria
- Integration test with a fixture that elicits input: exchange captured and replayable.

## Dependencies
- 17, 18, 24.

## Validation
- Integration test.

## Non-goals
- No LLM-generated answers (v1).

## Design References
- `docs/DESIGN.md` §2.3, §8.1, §11.5.
