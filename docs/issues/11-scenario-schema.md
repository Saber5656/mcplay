# 11 — Scenario zod schema & types

## Title
Scenario file schema (`src/core/scenario/schema.ts`) with zod as source of truth

## Summary
Define the `schemaVersion: "1"` scenario schema in zod exactly as specified in
`docs/DESIGN.md` §7.1, exporting inferred TypeScript types.

## Context
The scenario file is the central artifact and an untrusted-input boundary (ADR-006,
ADR-005 B2). zod is the single source of truth for validation and types.

## Scope
- zod schemas for: top-level scenario, `target`, `protocol`, `server`, `catalog`,
  `Interaction`, `redaction`.
- `.strict()` on objects to reject unknown fields.
- Exported inferred types (`Scenario`, `Interaction`, etc.).
- A `parseScenario(unknown): Scenario` that validates and returns typed data or throws
  `ValidationError` (exit 3).

## Detailed Requirements
- Field names/shapes MUST match DESIGN §7.1 exactly (`schemaVersion`, `target.envKeys`,
  `interactions[].direction`, `kind`, `timing`, `truncated`, `redacted`, etc.).
- `direction` ∈ {`client->server`,`server->client`}; `kind` ∈ {`request-response`,
  `notification`}; `transport` literal `stdio` in v1.
- `catalog.resources/resourceTemplates/prompts` nullable (populated only by Wave 8).
- Validation rejects unknown top-level and nested fields.

## Acceptance Criteria
- Unit tests: a valid fixture parses; unknown-field and wrong-type fixtures throw
  `ValidationError`.
- Types compile and are importable by store/matcher.

## Validation
- Unit tests with valid + invalid fixtures.

## Dependencies
- 01.

## Non-goals
- No file IO (Issue 13), no migration (Issue 14), no capability payloads beyond structure.

## Design References
- `docs/DESIGN.md` §7.1; ADR-006; ADR-005 B2.
