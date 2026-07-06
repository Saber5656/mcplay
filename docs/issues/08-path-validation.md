# 08 — Path validation module

## Title
Path validation & traversal/symlink guards (`src/core/security/path.ts`)

## Summary
Centralized validation that any input/output path stays within an allowed base directory,
rejecting `..` traversal and symlink escapes.

## Context
ADR-005 B2/B5: scenario/script/config/cwd paths derive from user or file input and must not
read or write outside intended directories.

## Scope
- `resolveWithin(base: string, candidate: string): string` returns a normalized absolute
  path or throws `SecurityPolicyError`.
- `assertNoSymlinkEscape(path, base)` checks real path stays under base.
- Helpers for the scenarios dir, `--out`, config path, and spawn cwd.

## Detailed Requirements
- Reject paths that resolve outside `base` (exit code 5).
- Resolve symlinks (`fs.realpath`) and re-check containment.
- Do not create directories implicitly except where a write is intended and confirmed.

## Acceptance Criteria
- Unit test: `../../etc/passwd` under a base is rejected.
- Unit test: a symlink pointing outside base is rejected.

## Validation
- Unit tests; consumed by store/config/spawner.
- Security acceptance suite (Issue 47).

## Dependencies
- 01, 02.

## Non-goals
- No OS-level sandboxing.

## Design References
- `docs/DESIGN.md` §11.4; ADR-005.
