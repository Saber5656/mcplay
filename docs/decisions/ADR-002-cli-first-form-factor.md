# ADR-002: CLI-First Form Factor

- Status: Accepted
- Date: 2026-07-06
- Deciders: Human user + Fable (design)

## Context

mcplay's primary interface must be chosen before command and module boundaries are drawn.
Options ranged from a terminal CLI/TUI, a local web dashboard, an embeddable library, to a
desktop app.

## Decision

**v1 is a CLI-first tool.** The one-and-only user surface for v1 is the `mcplay` command
(subcommands + interactive REPL for record mode). A lightweight local web UI is a **v2**
item. A programmatic library API is exposed only incidentally (internal modules), not as a
supported public API in v1.

## Rationale

- The target users are MCP server developers who live in the terminal.
- CLI is the fastest path to a shippable, testable v1 with the smallest attack surface (no
  HTTP server, no browser, no auth).
- A local web UI can be layered on the same core modules later without rework, provided the
  core (recorder, replay, scenario store) is UI-agnostic.

## Consequences

- Core logic MUST be implemented as UI-agnostic modules (`src/core/**`) so a future web UI
  reuses them. CLI code (`src/cli/**`) is a thin adapter.
- Interactive record mode uses a terminal REPL; it must degrade gracefully in
  non-interactive contexts (piped stdin, CI) by supporting scripted input.
- No web server, no localhost port, no browser sandbox concerns in v1.

## Alternatives considered

| Option | Why not for v1 |
|---|---|
| Local web UI first | Higher implementation cost; adds a localhost HTTP boundary and browser security concerns. Deferred to v2. |
| Library/SDK first | Weaker standalone product experience; API surface stabilization cost. |
| Desktop app (Electron/Tauri) | Large distribution, signing, update, and attack-surface cost; disproportionate for v1. |
