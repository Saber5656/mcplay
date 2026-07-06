# 23 — Replay matching engine

## Title
Replay matching engine (`src/core/replay/match.ts`)

## Summary
Given an incoming replay request, select the recorded interaction to answer with, supporting
`strict`, `sequential`, and `best-effort` modes (DESIGN §9.2).

## Context
The mock server (Issue 24) delegates request→response selection to this engine.

## Scope
- `createMatcher(scenario, mode): Matcher` with `match(method, params): MatchResult`.
- `strict`: exact canonical (method+params) match via Issue 12; repeated identical
  recordings returned in order (stateful cursor); no match → `unmatched` (JSON-RPC error
  `-32001 mcplay/unmatched`).
- `sequential`: next recorded interaction for that method, in order, ignoring params.
- `best-effort`: exact match else nearest same-method by param similarity; flag approximate.
- `--strict-unmatched` forces error on any miss.

## Detailed Requirements
- Deterministic ordering for repeated matches (cursor per canonical key).
- Log every decision at `debug` (method, mode, matched index, approximate?).
- Pure with respect to the scenario (no mutation of scenario data).

## Acceptance Criteria
- Unit tests for each mode: exact hit, repeated-in-order, miss→error, sequential ordering,
  best-effort fallback + approximate flag.

## Dependencies
- 11, 12.

## Validation
- Unit tests.

## Non-goals
- No server IO (Issue 24). No capability-specific logic beyond method+params.

## Design References
- `docs/DESIGN.md` §9.2, §9.3.
