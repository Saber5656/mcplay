# ADR-001: Language and Runtime — TypeScript on Node.js

- Status: Accepted
- Date: 2026-07-06
- Deciders: Human user + Fable (design)

## Context

mcplay is an MCP client harness / mock-server tool intended to be released as an
open-source, developer-facing CLI. The implementation language determines SDK maturity,
distribution channel, contributor pool, and type-safety guarantees for the scenario
schema.

## Decision

Implement mcplay in **TypeScript**, targeting the **Node.js** runtime, distributed via
**npm** (`npx mcplay ...` and global install).

- Node.js LTS baseline: **Node 20+** (v1). Declared in `package.json` `engines`.
- Language target: TypeScript compiled to ESM.
- Distribution: npm package with a `bin` entry named `mcplay`.

## Rationale

- The official MCP TypeScript SDK (`@modelcontextprotocol/sdk`) is the most mature SDK and
  matches the ecosystem mcplay operates in.
- npm/npx distribution is idiomatic for MCP tooling and requires no compilation step for
  end users.
- TypeScript's static types let the scenario schema, matching engine, and CLI contracts be
  strongly typed, reducing implementation ambiguity for lower-capability agents.

## Alternatives considered

| Option | Why not |
|---|---|
| Python (MCP Python SDK, uvx) | Viable, but weaker static typing for the scenario schema and a second-class SDK relative to TS for client+server symmetry. |
| Go / Rust single binary | Better single-binary distribution and performance, but less mature MCP SDKs and higher implementation cost for a v1. |

## Consequences

- Single-binary distribution (a v2 idea) will require an extra bundling step (e.g. `pkg`,
  Bun, or Node SEA). Documented as a v2 item, not v1.
- Runtime requires Node 20+ on the user's machine; documented as a prerequisite.
- The scenario schema is defined once in TypeScript (via zod) and is the single source of
  truth for validation and types.
