# 52 — npm release/publish workflow + provenance

## Title
npm release/publish workflow with provenance

## Summary
Automate versioned npm publishing with the full test gate and npm provenance, triggered by a
release tag.

## Context
Distribution is via npm (ADR-001). Release must be gated on the whole-product validation
suite and produce provenance for supply-chain trust (ADR-005 §11.7).

## Scope
- `.github/workflows/release.yml`: on tag `v*`, run build/typecheck/lint/test (incl. security
  + round-trip suites) then `npm publish --provenance`.
- Configure `package.json` `files` allowlist (ship `dist/` + docs; exclude tests/fixtures).
- Versioning policy documented (semver; `schemaVersion` independent of package version).

## Detailed Requirements
- Publish only if all gates pass.
- Least-privilege token via OIDC where possible; no long-lived token in logs.
- `prepublishOnly` runs build; `.npmignore`/`files` excludes `test/**` and fixtures.

## Acceptance Criteria
- A dry-run (`npm publish --dry-run`) in CI produces the expected file set.
- Release workflow is valid, gated on tests, and requests provenance.

## Dependencies
- 01, 05, 51.

## Validation
- CI dry-run; manual first release by maintainer.

## Non-goals
- No auto-changelog generation in v1 (may be a follow-up).

## Design References
- `docs/DESIGN.md` §11.7, §15, §16; ADR-001, ADR-005.
