# 47 — Security acceptance test suite

## Title
Security acceptance test suite

## Summary
A dedicated test suite that proves each non-negotiable secure default from ADR-005 /
DESIGN §11 holds, so implementation agents cannot silently regress security.

## Context
Security is a core requirement. These tests are the gate that the trust-boundary controls
actually work end-to-end (DESIGN §15 "Security tests").

## Scope
Tests (each must exist and pass):
- Shell injection inert: a command with shell metacharacters is not shell-expanded (Issue 06).
- Env isolation: a secret var in the parent env is NOT visible to the spawned server (07).
- Env values never persisted into scenarios (07/21).
- Embedded-command refusal: loading/replaying a scenario never executes `target.command`;
  execution requires `--allow-command` + confirmation (10, 24, 25).
- File-parse limits: oversized and deeply-nested scenario/script files are refused (09/13).
- Path traversal / symlink escape refused for input and output paths (08).
- Redaction efficacy: seeded secrets are removed; `--no-redact` warns and marks the file (21).
- Export gate: unredacted secret-bearing scenario blocked without confirmation (35).

## Detailed Requirements
- Tests assert observable behavior (exit codes, absence of execution/leak), not
  implementation internals.
- Run in CI on every PR (Issue 05).

## Acceptance Criteria
- All listed tests present and green; a deliberately weakened control makes the relevant test
  fail (verified during review).

## Dependencies
- 06, 07, 08, 09, 10, 15, 21 (and the commands they gate).

## Validation
- CI-run suite.

## Non-goals
- No network/SSRF tests (v2). No fuzzing harness in v1 (may be a follow-up).

## Design References
- `docs/DESIGN.md` §11, §15; ADR-005.
