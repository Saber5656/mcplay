# 32 — `mcplay ls` command

## Title
`mcplay ls` command (`src/cli/commands/ls.ts`)

## Summary
List scenario files in the scenarios directory with brief metadata.

## Context
Users need to discover local scenarios (DESIGN §6). No remote listing in v1 (ADR-004).

## Scope
- `mcplay ls` flags: `--dir <path>`, `--json`.
- For each `*.mcplay.json` in the dir: show name, id (short), server name/version, createdAt,
  interaction count, redaction status.

## Detailed Requirements
- Dir defaults to config `scenariosDir`; path validated (Issue 08).
- Skip/flag files that fail schema validation rather than crashing.
- `--json` returns an array of summaries.

## Acceptance Criteria
- Test: a dir with valid + one invalid scenario lists valid ones and flags the invalid one.

## Dependencies
- 13, 16.

## Validation
- CLI tests.

## Non-goals
- No recursive/remote listing.

## Design References
- `docs/DESIGN.md` §6; ADR-004.
