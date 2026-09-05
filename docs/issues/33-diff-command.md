# 33 — Scenario diff engine + `mcplay diff`

## Title
Scenario diff engine and command (`src/core/diff/diff.ts`, `src/cli/commands/diff.ts`)

## Summary
Compare two scenarios structurally (catalog, capabilities, interactions) and present a
readable diff.

## Context
Useful for regression review (recorded vs re-recorded) and understanding server changes
(DESIGN §3, §6).

## Scope
- `diffScenarios(a, b): DiffReport` comparing server info/capabilities, tool catalog, and
  interactions keyed by canonical (method+params) (Issue 12).
- CLI `mcplay diff <a> <b>` with `--json`.

## Detailed Requirements
- Report added/removed/changed tools and interactions with canonical keys.
- Value comparison uses canonicalization; redacted placeholders compare equal to themselves.
- Human output is a compact, colorized (unless `--no-color`) summary; `--json` is structured.

## Acceptance Criteria
- Unit tests: identical scenarios → empty diff; a changed tool result → a `changed` entry;
  added/removed tools detected.

## Dependencies
- 11, 12, 13.

## Validation
- Unit + CLI tests.

## Non-goals
- No 3-way merge. No patching.

## Design References
- `docs/DESIGN.md` §6, §7.1, §9.2.
