# 50 — SECURITY.md + threat model

## Title
SECURITY.md and threat-model documentation

## Summary
Publish the security policy (disclosure process) and a user-facing threat model derived from
ADR-005 / DESIGN §11.

## Context
Public OSS projects need a disclosure policy and a clear statement of trust boundaries and
guarantees/limitations.

## Scope
- `SECURITY.md`: supported versions, private disclosure contact/process, response
  expectations.
- `docs/THREAT_MODEL.md`: trust boundaries (B1–B5), threats, controls, and explicit v1
  limitations (no sandbox beyond env/limits; stdio-only; secret redaction is best-effort).

## Detailed Requirements
- Threat model mirrors DESIGN §11 and states residual risks honestly (e.g., a spawned server
  still runs with the user's privileges; redaction is heuristic).
- Disclosure contact is a placeholder for the human to fill (do not invent an address).

## Acceptance Criteria
- Both files exist; threat model enumerates B1–B5 with controls and limitations.

## Dependencies
- ADR-005.

## Validation
- Review against DESIGN §11 for completeness.

## Non-goals
- No formal audit in v1.

## Design References
- `docs/DESIGN.md` §11; ADR-005.
