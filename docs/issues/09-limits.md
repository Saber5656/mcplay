# 09 — Limits (timeouts / size / depth) module

## Title
Resource limits module (`src/core/security/limits.ts`)

## Summary
Provide the timeout, payload-size, file-size, and JSON-depth limits used across spawning,
capture, and file loading, with config-overridable defaults.

## Context
ADR-005 §11.6: a malicious or buggy server/file must not hang mcplay or exhaust memory.

## Scope
- Default constants: `requestTimeoutMs=30000`, session timeout, `maxPayloadBytes=1048576`,
  `maxFileBytes=16777216`, `jsonDepth=64`.
- `withTimeout(promise, ms)`; `capBytes(buffer, max) -> { data, truncated }`;
  `assertJsonDepth(value, max)`; `assertFileSize(bytes, max)`.

## Detailed Requirements
- Limits are read from config (Issue 16) with these defaults; CLI flags override.
- `capBytes` returns a truncation marker so callers set `interaction.truncated=true`.
- Depth check runs before full parse where possible (streaming or post-parse bounded).

## Acceptance Criteria
- Unit tests for each helper, including truncation and depth rejection.

## Validation
- Unit tests; consumed by 06, 13, 18.
- Security acceptance suite (Issue 47) exercises oversized/deep inputs.

## Dependencies
- 01, 02.

## Non-goals
- No global memory accounting beyond per-payload/file caps.

## Design References
- `docs/DESIGN.md` §11.6, §14; ADR-005.
