# ADR-006: Scenario File Format

- Status: Accepted
- Date: 2026-07-06
- Deciders: Human user + Fable (design)

## Context

The scenario file is the central artifact: it is produced by recording, consumed by
replay, and exchanged when sharing. Because sharing is file-based (ADR-004), the format is
a public, versioned contract and an untrusted-input boundary (ADR-005).

## Decision

- **Format:** JSON (UTF-8), single file with extension **`.mcplay.json`**.
- **Top-level `schemaVersion`:** a monotonic string, starting at `"1"`. Every reader checks
  it first and refuses unknown major versions unless a migration exists.
- **Canonical schema:** defined once in TypeScript using **zod** in `src/core/scenario/schema.ts`;
  this is the single source of truth for both validation and TypeScript types.
- **Determinism:** files are written with stable key ordering and 2-space indentation so
  diffs are meaningful and git-friendly.
- **Integrity:** `export` computes a SHA-256 checksum stored alongside/in the bundle;
  `import`/`validate` verify it when present.
- **No executable content:** the format contains data only. Any `command`/`args` present
  describe how the scenario was recorded and are treated as untrusted (see ADR-005).

## Rationale

- JSON is universally diffable, reviewable, and supported; YAML's implicit typing and
  anchors add parsing risk for an untrusted-input format.
- A single file keeps sharing trivial (one attachment / one gist).
- zod-as-source-of-truth removes drift between validation and types.

## Consequences

- A migration framework (`src/core/scenario/migrate.ts`) handles `schemaVersion` upgrades.
- Large recorded payloads are truncated with explicit markers rather than allowed to bloat
  files unbounded (size caps from ADR-005).
- Script files (for `mcplay run`) are a **separate** format (`*.mcplay.script.json`),
  documented in `docs/DESIGN.md` §Script Format, not conflated with scenario files.

## Alternatives considered

| Option | Why not |
|---|---|
| YAML | Implicit typing, anchors/aliases, and merge keys increase parser attack surface. |
| Directory bundle (multi-file) | Harder to share as one unit; deferred unless payload sizes demand it. |
| Binary/compressed | Not human-diffable or reviewable; hostile to git-based sharing. |
