# 49 — User documentation

## Title
User documentation (README + usage guides)

## Summary
Write end-user documentation: install, quickstart (record → replay → share), full command
reference, config reference, and security notes.

## Context
An OSS tool needs usable docs for adoption; this consolidates the CLI surface (DESIGN §6) and
security expectations for users.

## Scope
- Update `README.md`: what mcplay is, install (`npx mcplay`), quickstart journeys J1–J4.
- `docs/USAGE.md`: per-command reference with flags and exit codes (mirror DESIGN §6).
- `docs/CONFIG.md`: config fields and defaults (mirror DESIGN §13).
- A short "security & sharing" section: redaction, embedded-command safety, what NOT to
  commit.

## Detailed Requirements
- Command reference must match the implemented CLI exactly (kept in sync with DESIGN §6).
- Include the tools-only MVP scope note and the capability roadmap (Wave 8).
- Examples use the bundled fixture or a well-known public server.

## Acceptance Criteria
- README quickstart commands are copy-pasteable and correct against the shipped CLI.
- Every implemented command appears in `docs/USAGE.md`.

## Dependencies
- 22, 25, 29, 30–37.

## Validation
- Doc lint / link check in CI (optional); manual review of examples.

## Non-goals
- No docs site generator in v1.

## Design References
- `docs/DESIGN.md` §6, §13, §16.
