# ADR-005: Security Model and Trust Boundaries

- Status: Accepted
- Date: 2026-07-06
- Deciders: Human user + Fable (design)

## Context

mcplay is intended for public open-source release. It executes external MCP server
commands as child processes, parses shared scenario files, and may observe secrets in
tool arguments/results and environment. Security is a core design requirement, not a later
hardening pass.

## Decision

Adopt an explicit trust-boundary model with secure defaults. The authoritative, detailed
version lives in `docs/DESIGN.md` §Security Model. This ADR fixes the non-negotiable
principles.

### Trust boundaries (v1)

1. **Child-process boundary** — spawning target MCP servers. Highest risk.
2. **Scenario-file parser boundary** — imported/loaded scenario files are untrusted input.
3. **Secret-handling boundary** — recorded data and environment may contain secrets.
4. **Replay output boundary** — mcplay-as-mock emits recorded data to a downstream client.
5. **Persistence boundary** — scenario files written to disk.

Network/SSRF boundaries are **out of scope for v1** (stdio-only, no backend) and become
relevant in v2 (Streamable HTTP, sync server).

### Non-negotiable secure defaults

- Spawn child processes with `shell: false` and an explicit `argv` array. **Never** pass a
  command through a shell string. No shell metacharacter interpolation anywhere.
- Do **not** inherit the full parent environment into spawned servers. Pass only an
  explicit allowlist of environment variables. Values are never written into scenarios by
  default (only key names, if any).
- Loading a scenario that contains an embedded server `command` MUST require explicit user
  confirmation before mcplay will execute it. A scenario file must never cause silent code
  execution.
- All scenario files are validated against the schema (zod) before use, with size limits,
  nesting-depth limits, and rejection of unknown/unsafe fields.
- All file paths derived from user or file input are validated to stay within an allowed
  base directory (no path traversal, no symlink escape).
- Redaction of common secret patterns is **on by default** for recorded content; secrets
  must be redactable before a scenario is exported/shared.
- Per-request and per-session timeouts and output-size caps are enforced to bound a
  malicious or buggy server.
- Fail closed: on any validation or policy failure, refuse the operation with a clear error
  rather than proceeding with reduced safety.

## Consequences

- A dedicated security module (`src/core/security/**`) centralizes command policy, env
  allowlist, path validation, and limits. Other modules MUST route through it.
- Dedicated security issues and security acceptance criteria exist so implementation agents
  cannot skip them (see `docs/ISSUE_PLAN.md`).
- Dependencies are minimized and pinned; supply-chain controls (Dependabot, CodeQL, lockfile,
  provenance) are part of the release posture.

## Alternatives considered

- Inheriting parent env for convenience — rejected; leaks secrets into untrusted servers.
- Executing scenario-embedded commands automatically — rejected; remote-code-execution risk
  from shared files.
