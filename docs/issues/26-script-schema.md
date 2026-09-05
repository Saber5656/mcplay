# 26 — Script schema

## Title
Script (steps) schema (`src/core/script/schema.ts`)

## Summary
Define the `*.mcplay.script.json` format (steps + optional expectations) in zod, per
DESIGN §7.2.

## Context
Scripts drive deterministic recording and assertion runs (`mcplay run`, `mcplay record
--script`). Kept separate from the scenario format (ADR-006).

## Scope
- zod schema: `{ schemaVersion:"1", name, steps: Step[] }`.
- MVP `Step` actions: `initialize`, `list_tools`, `call_tool { name, arguments, expect? }`.
- `expect`: `{ isError?, resultContains?, resultMatches?, errorCode? }`.
- `.strict()` objects; exported types + `parseScript()`.

## Detailed Requirements
- Field names exactly per DESIGN §7.2.
- Extended actions (`read_resource`, `get_prompt`) are added in Issue 46 — schema must be
  designed to extend without breaking MVP scripts.
- Validation errors are `ValidationError` (exit 3).

## Acceptance Criteria
- Unit tests: valid script parses; unknown action/field rejected.

## Dependencies
- 11 (zod patterns).

## Validation
- Unit tests with valid + invalid fixtures.

## Non-goals
- No execution (Issues 28, 30). No resources/prompts actions (Issue 46).

## Design References
- `docs/DESIGN.md` §7.2, §10; ADR-006.
