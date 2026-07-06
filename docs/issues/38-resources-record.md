# 38 — Resources: client ops + record

## Title
Resources capability — client operations & recording

## Summary
Extend the client harness and recorder to support the MCP resources capability:
`resources/list`, `resources/read`, `resources/templates/list`, and `resources/subscribe` +
`notifications/resources/updated`.

## Context
Post-MVP capability, issued separately per the human decision. Depends on the tools path
(17/19) being stable.

## Scope
- Client ops: `listResources`, `readResource`, `listResourceTemplates`, `subscribe`.
- Recorder: capture resource interactions and snapshot `catalog.resources` +
  `catalog.resourceTemplates`.
- Extend record REPL with `resources`, `read <uri>` verbs.

## Detailed Requirements
- Snapshot resources/templates on `ready` and refresh on `notifications/resources/list_changed`.
- Capture `resources/updated` notifications attached to the relevant interaction.
- Redaction applies to resource contents (Issue 21).

## Acceptance Criteria
- Integration test against a resources-enabled fixture: list/read captured; catalog
  populated.

## Dependencies
- 17, 19, 11.

## Validation
- Integration tests (extend fixture with resources).

## Non-goals
- Replay/mock for resources is Issue 39.

## Design References
- `docs/DESIGN.md` §2.3, §7.1 (catalog), §8; research/mcp-protocol-grounding.md.
