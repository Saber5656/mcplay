# 03 — Structured logging module

## Title
Structured, level-controlled logging module

## Summary
Implement `src/core/logging/logger.ts`: leveled logging (`error|warn|info|debug`) writing to
stderr, never to stdout (stdout is reserved for protocol/data output).

## Context
Record/replay use stdout for MCP JSON-RPC and machine output; logs must go to stderr to
avoid corrupting protocol streams. Match decisions and security events must be observable at
`debug`.

## Scope
- Logger factory with level from config/`--log-level`.
- Secret-safe: a `redactValue`-aware formatter that never prints values flagged secret.
- Structured fields support (object context) with a human and `--json` line format.

## Detailed Requirements
- Default level `info`. All output to `process.stderr`.
- MUST NOT write to stdout (guard/documented) so `replay` stdio stays clean.
- Provide `log.security(...)` at `warn`+ for policy events (embedded-command refusal, etc.).

## Acceptance Criteria
- Unit test: nothing is written to stdout by the logger.
- Level filtering works; `debug` hidden at `info`.

## Validation
- Unit tests capturing stderr/stdout.

## Dependencies
- 01.

## Non-goals
- No file/rotating logs in v1.

## Design References
- `docs/DESIGN.md` §9 (stdio cleanliness), §11.5 (never log secrets).
