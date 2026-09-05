# 46 — Script actions & assertions for resources/prompts

## Title
Extended script actions & assertions (resources/prompts)

## Summary
Extend the script schema, runner, and assertion engine with `read_resource` and `get_prompt`
actions and matching expectations.

## Context
Once resources/prompts record+replay exist (38–41), scripts should drive and assert them too
(DESIGN §7.2, §10).

## Scope
- Add `read_resource { uri, expect? }` and `get_prompt { name, arguments, expect? }` to the
  script schema (extend Issue 26 without breaking MVP scripts).
- Runner executes these via the resources/prompts client ops (38/40).
- Assertions: `resultContains`/`resultMatches`/`isError` applied to resource contents and
  prompt messages.

## Detailed Requirements
- Backward-compatible schema extension (existing tools-only scripts still valid).
- Deterministic execution and clear assertion failures.

## Acceptance Criteria
- Integration test: a script reading a resource and getting a prompt asserts successfully
  against the extended fixture.

## Dependencies
- 26, 27, 28, 38, 40.

## Validation
- Integration tests.

## Non-goals
- No sampling/elicitation scripting in v1 (interactive/stub only).

## Design References
- `docs/DESIGN.md` §7.2, §10, §2.3.
