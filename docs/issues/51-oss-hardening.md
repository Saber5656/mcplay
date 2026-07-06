# 51 — OSS repository hardening config

## Title
OSS repository hardening (Dependabot, CodeQL, secret scanning, CODEOWNERS)

## Summary
Add supply-chain and repository-security configuration for public release per ADR-005 §11.7.

## Context
Public OSS release requires baseline supply-chain and repo hardening.

## Scope
- `.github/dependabot.yml` (npm + github-actions ecosystems, scheduled).
- `.github/workflows/codeql.yml` (JS/TS analysis, least-privilege, pinned).
- Enable secret scanning + push protection (documented steps if not settable via files).
- `CODEOWNERS`, issue/PR templates, `CONTRIBUTING.md` stub.
- Document required branch-protection ruleset (no direct push to default, PR + review, no
  force-push) as a repo setting the maintainer applies.

## Detailed Requirements
- All workflows least-privilege and SHA-pinned.
- Where a control is a GitHub setting (not a file), document the exact steps in
  `docs/RELEASE.md` or `CONTRIBUTING.md`.

## Acceptance Criteria
- Dependabot + CodeQL configs valid; CodeQL runs green.
- Branch-protection and secret-scanning steps documented.

## Dependencies
- 01, 05.

## Validation
- CI validates CodeQL workflow; manual verification of settings docs.

## Non-goals
- No paid security tooling.

## Design References
- `docs/DESIGN.md` §11.7; ADR-005.
