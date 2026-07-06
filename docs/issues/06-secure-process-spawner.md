# 06 — Secure child-process spawner

## Title
Secure child-process spawner & lifecycle manager (`src/core/mcp/process.ts`)

## Summary
Implement the single, centralized way mcplay spawns a target MCP server: `shell:false`,
argv array, env allowlist, cwd validation, timeouts, output caps, and guaranteed teardown.

## Context
The child-process boundary (ADR-005 B1) is the highest-risk boundary. All spawning MUST go
through this module; no other module may call `child_process` directly.

## Scope
- `spawnServer({ command, args, env, cwd, limits }): SpawnedServer` returning stdio streams
  and a `dispose()`.
- Uses `child_process.spawn` with `{ shell: false }` and an explicit argv array.
- Env comes only from `security/env.ts` (Issue 07); never `process.env` wholesale.
- cwd validated via `security/path.ts` (Issue 08).
- Enforces limits from `security/limits.ts` (Issue 09): session timeout, per-request
  timeout hooks, and byte caps on captured output.
- On dispose/exit: `SIGTERM`, then `SIGKILL` after a grace period; remove listeners.

## Detailed Requirements
- MUST NOT invoke a shell or interpolate a command string.
- MUST reject an empty/nonexistent command with a clear `RuntimeError` (ENOENT) before any
  side effects.
- Surfaces process exit/crash events so the recorder can mark incomplete recordings.
- No inheritance of parent stdio except the piped channels it manages.

## Acceptance Criteria
- Unit test: a metacharacter-laden "command" is treated as a literal argv[0] (no shell
  expansion) and fails as ENOENT rather than executing anything.
- Unit test: env passed to the child equals the allowlisted set only.
- Unit test: process is killed on `dispose()` and on session timeout.

## Validation
- Unit tests + used by Issue 17/19 integration.
- Security acceptance suite (Issue 47) references these behaviors.

## Dependencies
- 01, 02, 03, 09 (and 07, 08 for env/cwd wiring).

## Non-goals
- No HTTP transport (v2). No sandboxing beyond env/limits in v1 (documented).

## Design References
- `docs/DESIGN.md` §11.1 (B1), §11.6, §14; ADR-005.
