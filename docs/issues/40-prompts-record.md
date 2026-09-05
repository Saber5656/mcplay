# 40 — Prompts: client ops + record

## Title
Prompts capability — client operations & recording

## Summary
Extend the client harness and recorder to support `prompts/list` and `prompts/get`.

## Context
Post-MVP capability, issued separately. Depends on the tools path (17/19).

## Scope
- Client ops: `listPrompts`, `getPrompt(name, arguments)`.
- Recorder: capture prompt interactions; snapshot `catalog.prompts`.
- Extend REPL with `prompts`, `prompt <name> <json-args>` verbs.

## Detailed Requirements
- Snapshot prompts on `ready`; refresh on `notifications/prompts/list_changed`.
- Redaction applies to prompt messages/args (Issue 21).

## Acceptance Criteria
- Integration test against a prompts-enabled fixture: list/get captured; catalog populated.

## Dependencies
- 17, 19, 11.

## Validation
- Integration tests (extend fixture with prompts).

## Non-goals
- Replay/mock for prompts is Issue 41.

## Design References
- `docs/DESIGN.md` §2.3, §7.1, §8.
