# 19 — Recorder core & session state machine

## Title
Recorder core and session state machine (`src/core/recorder/recorder.ts`)

## Summary
Orchestrate a recording session: spawn+connect, capture catalog and interactions, handle
crashes/timeouts, and assemble a `Scenario` object (not yet persisted).

## Context
Implements the record state machine in DESIGN §8.2. Coordinates client (17), capture (18),
catalog (20), and redaction (21).

## Scope
- State machine: `idle→spawning→initializing→ready→(drive)*→finalizing→closed`, with
  `error` transitions.
- API: `startRecording(target, limits)`, `drive(action)` (list/call/raw), `finalize(): Scenario`.
- On server crash: mark scenario metadata `incomplete`, still return a valid `Scenario`.
- On spawn failure: throw; produce no scenario.

## Detailed Requirements
- Capture `initialize` + `notifications/initialized` first; snapshot catalog on `ready`
  (Issue 20) and on `tools/list_changed`.
- Enforce session timeout; per-request timeout via client.
- Apply redaction (Issue 21) during `finalize` per config (`--no-redact` opt-out warns and
  sets `redaction.applied=false`).
- Always dispose the child on any terminal state.

## Acceptance Criteria
- Integration test: record `add`/`echo` calls against fixture → a schema-valid `Scenario`
  with correct interactions and catalog.
- Test: simulated crash yields an `incomplete`-marked scenario; spawn failure yields none.

## Dependencies
- 17, 18, 20, 21.

## Validation
- Integration tests with fixture server.

## Non-goals
- No CLI/REPL (Issue 22). No file IO (Issue 13, invoked by the command).

## Design References
- `docs/DESIGN.md` §8; §14; ISSUE_PLAN KU5.
