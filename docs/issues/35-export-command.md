# 35 — `mcplay export` command

## Title
`mcplay export` command (`src/cli/commands/export.ts`)

## Summary
Produce a shareable scenario artifact: normalized serialization, integrity checksum, and a
redaction gate that blocks/ warns before potentially-secret data leaves the machine.

## Context
Sharing is file-based (ADR-004); export is the safety checkpoint before a file is handed to
someone else (DESIGN §11.5).

## Scope
- `mcplay export <scenario>` flags: `-o/--out`.
- Steps: load+validate (13), lint (15), redaction gate, normalize serialization, compute
  SHA-256 and embed/emit alongside, write output.

## Detailed Requirements
- Redaction gate: if `redaction.applied=false` or lint finds likely secrets, refuse unless
  the user confirms (or runs `redact` first); log prominently.
- Checksum over the canonical serialization; recorded in the artifact/metadata.
- Deterministic output for reproducibility.

## Acceptance Criteria
- Test: exporting an unredacted secret-bearing scenario is blocked without confirmation.
- Test: exported file carries a valid checksum verified by `import`.

## Dependencies
- 13, 15, 21.

## Validation
- CLI tests.

## Non-goals
- No upload/network (ADR-004).

## Design References
- `docs/DESIGN.md` §11.5; ADR-004, ADR-006.
