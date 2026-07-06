# 07 — Environment allowlist module

## Title
Environment allowlist resolution (`src/core/security/env.ts`)

## Summary
Compute the environment a spawned server receives: a minimal safe base plus an explicit
per-run allowlist. Never inherit the full parent environment.

## Context
ADR-005 §11.3: leaking the parent env into an untrusted server risks exposing unrelated
secrets. Env values must never be persisted into scenarios.

## Scope
- `resolveEnv(allow: string[], overrides: Record<string,string>): { env, keys }`.
- Safe base: `PATH`, `HOME`, `TMPDIR`, `LANG`/`LC_*` (documented constant list).
- `--env KEY` inherits that var's value from the parent; `--env KEY=VALUE` sets it.
- Returns `keys` (names only) for storage in `scenario.target.envKeys`.

## Detailed Requirements
- MUST NOT copy `process.env` wholesale.
- MUST NOT return values for storage — only names via `keys`.
- Unknown/empty keys ignored with a warning.

## Acceptance Criteria
- Unit test: default env excludes an arbitrary secret var present in the parent env.
- Unit test: `keys` contains names, and no values leak into any persisted structure.

## Validation
- Unit tests; consumed by Issue 06.
- Security acceptance suite (Issue 47).

## Dependencies
- 01, 02.

## Non-goals
- No secret-store integration.

## Design References
- `docs/DESIGN.md` §11.3; ADR-005.
