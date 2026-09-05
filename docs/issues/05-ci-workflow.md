# 05 — Hardened CI workflow

## Title
GitHub Actions CI: build, typecheck, lint, test (hardened)

## Summary
Add a hardened CI workflow running build, typecheck, lint, and tests on Node 20 and 22.

## Context
CI is the enforcement point for the whole-product validation strategy and part of the OSS
security posture (ADR-005 §11.7).

## Scope
- `.github/workflows/ci.yml`: matrix Node 20 + 22; steps `npm ci`, `build`, `typecheck`,
  `lint`, `test` (with coverage).
- Least-privilege `permissions: contents: read`.
- Pin actions to full commit SHAs.
- Cache npm; run on PRs and pushes to non-default branches.

## Detailed Requirements
- No secrets exposed to PR jobs from forks.
- Fail the job on any step failure; upload coverage as an artifact (no external upload
  service in v1).
- `concurrency` to cancel superseded runs.

## Acceptance Criteria
- Workflow is valid and green on the scaffolding.
- `permissions` are least-privilege; actions pinned by SHA.

## Validation
- CI run passes on the branch.

## Dependencies
- 01.

## Non-goals
- No publish job (Issue 52), no CodeQL/Dependabot here (Issue 51).

## Design References
- `docs/DESIGN.md` §11.7, §15; ADR-005.
