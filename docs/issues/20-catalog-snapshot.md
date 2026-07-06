# 20 — Catalog snapshot (tools)

## Title
Server catalog snapshot — tools (`src/core/recorder/catalog.ts`)

## Summary
Capture the server's advertised tool catalog (`tools/list`) into `scenario.catalog.tools`,
refreshing on `notifications/tools/list_changed`.

## Context
Replay needs the tool list to answer `tools/list` (DESIGN §9.1). MVP is tools-only;
resources/prompts snapshots are Wave 8.

## Scope
- `snapshotTools(client): Tool[]` capturing the verbatim `tools/list` result.
- Hook to re-snapshot on `notifications/tools/list_changed`.
- Store into `scenario.catalog.tools`.

## Detailed Requirements
- Store tool definitions verbatim (name, description, inputSchema) as returned.
- Handle pagination if the server paginates `tools/list` (accumulate all pages).
- Leave `catalog.resources/prompts` null in MVP.

## Acceptance Criteria
- Integration test: fixture's `echo`/`add` appear in the snapshot with their input schemas.
- Test: a `list_changed` notification triggers a refresh.

## Dependencies
- 11, 17.

## Validation
- Integration tests.

## Non-goals
- No resources/prompts catalog (Issues 38, 40).

## Design References
- `docs/DESIGN.md` §8.2, §9.1.
