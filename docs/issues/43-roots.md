# 43 — Roots (client capability) record + mock

## Title
Roots capability — record & mock (`roots/list`, `notifications/roots/list_changed`)

## Summary
Support the client-provided roots capability: record how roots were presented and reproduce
them on replay.

## Context
Roots let a server discover client-exposed filesystem roots. mcplay-as-harness advertises a
configured/stub roots set; mcplay-as-mock reproduces recorded `roots/list` responses.

## Scope
- Recorder: capture `roots/list` requests from the server and the harness's returned roots
  (a configured, non-sensitive stub by default), plus `list_changed` notifications.
- Mock: answer `roots/list` from the recording; emit recorded `list_changed`.
- Advertise roots capability only if recorded.

## Detailed Requirements
- Default harness roots are a safe stub (no real host paths) unless the user explicitly
  configures roots; never expose arbitrary host paths by default (security).
- Deterministic reproduction on replay.

## Acceptance Criteria
- Integration test: a fixture server requesting `roots/list` receives the stub and the
  exchange is captured and replayable.

## Dependencies
- 17, 24.

## Validation
- Integration test.

## Non-goals
- No real filesystem root exposure by default.

## Design References
- `docs/DESIGN.md` §2.3, §11 (safe defaults).
