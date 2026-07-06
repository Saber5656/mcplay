# 31 — `mcplay inspect` command

## Title
`mcplay inspect` command (`src/cli/commands/inspect.ts`)

## Summary
Print a human-readable summary of a scenario: target, server info, negotiated protocol,
tool catalog, and an interaction list.

## Context
Reviewers/educators need to understand a scenario without reading raw JSON (DESIGN §3 P3, §6).

## Scope
- `mcplay inspect <scenario>` flags: `--json`.
- Summary: name/id/createdAt, target command+args, server name/version, protocol versions,
  capability summary, tool count + names, interaction count by method, redaction status.

## Detailed Requirements
- Redaction-aware: never surface redacted values; show placeholders.
- `--json` returns the structured summary (not the whole file).
- Human output is concise and aligned (tabular where sensible).

## Acceptance Criteria
- Test: inspecting a fixture scenario shows correct counts and names in both formats.

## Dependencies
- 11, 13.

## Validation
- CLI tests.

## Non-goals
- No mutation. No diff (Issue 33).

## Design References
- `docs/DESIGN.md` §3, §6, §7.1.
