# 12 — Canonicalization utility

## Title
Canonical form for method + params (`src/core/util/canonical.ts`)

## Summary
Produce a deterministic canonical string/key from a JSON-RPC `method` + `params`, used by
the matching engine and diff.

## Context
Replay matching (DESIGN §9.2) and diff (§6 `diff`) need a stable key that is invariant to
key ordering and insignificant whitespace, while remaining faithful for value comparison.

## Scope
- `canonicalize(value): string` — stable JSON with sorted object keys, arrays preserved in
  order, numbers normalized, unicode normalized (NFC).
- `matchKey(method, params): string`.

## Detailed Requirements
- Object keys sorted lexicographically; arrays order-significant (documented; see KU3).
- Numbers serialized canonically (no `-0`, consistent float formatting).
- Deterministic across runs and platforms.
- Document edge cases (floats, nested arrays, unicode) as noted in ISSUE_PLAN KU3.

## Acceptance Criteria
- Unit tests: reordered object keys produce identical keys; reordered array elements produce
  different keys; unicode NFC equivalence holds.

## Validation
- Unit tests; consumed by 23, 27, 33.

## Dependencies
- 01.

## Non-goals
- No fuzzy/similarity scoring (that lives in the matcher's best-effort mode, Issue 23).

## Design References
- `docs/DESIGN.md` §7.1 (normalization), §9.2; ISSUE_PLAN KU3.
