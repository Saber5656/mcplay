# ADR-004: Local-File Sharing, No Backend (v1)

- Status: Accepted
- Date: 2026-07-06
- Deciders: Human user + Fable (design)

## Context

"Sharing" could mean local file export/import, a self-hostable sync server, or a hosted
public hub with accounts. Each adds progressively more scope and security surface
(auth, storage, moderation, abuse handling).

## Decision

**v1 sharing is local file input/output only.** Scenarios are portable files that users
exchange via existing channels (git, gist, file attachment). mcplay provides
`export`/`import` for normalization and integrity, but **no network service, no accounts,
no hosted registry** in v1.

## Rationale

- Keeps the v1 attack surface minimal: no server to secure, no auth, no stored user data,
  no abuse/moderation obligations.
- Portable files are sufficient for the core value (reproduce and share a server's
  behavior).
- A sync server or hub can be added later without breaking the file format.

## Consequences

- The scenario **file format is a public contract** and must be versioned, validated, and
  security-linted on import (see ADR-006 and ADR-005), because imported files are untrusted.
- No discovery/search of remote scenarios in v1.
- `export`/`import` do local operations only: format normalization, integrity checksum,
  and a redaction gate that warns before a file that may contain secrets leaves the machine.

## Alternatives considered

| Option | Why not for v1 |
|---|---|
| Local + self-hostable sync server | Adds auth, persistence, abuse controls; deferred to v2. |
| Hosted public hub with accounts | Full service operation, moderation, and account security; far too large for v1 OSS. |
