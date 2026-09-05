# 10 — Command policy & embedded-command gate

## Title
Command policy and embedded-command confirmation gate (`src/core/security/command-policy.ts`)

## Summary
Enforce that a target server command supplied on the CLI is allowed, while a command
embedded inside a scenario/script file is never executed without an explicit flag and
confirmation.

## Context
ADR-005 §11.2: shared scenario files are untrusted and may embed a malicious `command`.
Replay never needs it. Any execution of a file-embedded command is a potential RCE from a
shared file.

## Scope
- `assertCliCommandAllowed(argv)` — validates a non-empty argv provided after `--`.
- `gateEmbeddedCommand({ allowFlag, assumeYes, confirmFn })` — refuses (exit 5) unless
  `allowFlag` set; requires interactive confirmation unless `assumeYes && allowFlag`, and
  logs the decision prominently via `log.security`.

## Detailed Requirements
- Default behavior: refuse to execute any file-embedded command.
- `replay` MUST never call the embedded-command path (mock uses data only) — enforced by not
  wiring it there.
- No shelling out; policy only decides allow/refuse and returns argv to Issue 06.

## Acceptance Criteria
- Unit test: embedded command without `--allow-command` → `SecurityPolicyError` (exit 5).
- Unit test: with `--allow-command` and confirmation, argv is returned; the event is logged.

## Validation
- Unit tests; used by record/run when a script references a command.
- Security acceptance suite (Issue 47).

## Dependencies
- 01, 02, 08.

## Non-goals
- No persistent allowlist store in v1 (per-invocation only).

## Design References
- `docs/DESIGN.md` §11.2; ADR-005.
