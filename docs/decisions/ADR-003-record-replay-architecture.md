# ADR-003: Record + Mock-Replay Architecture

- Status: Accepted
- Date: 2026-07-06
- Deciders: Human user + Fable (design)

## Context

The README defines mcplay as "scenario recording, replay, and sharing." "Replay" is
ambiguous: it can mean (a) re-driving a live server with recorded requests (regression
testing) or (b) serving recorded responses as a mock server (offline reproduction). These
imply very different architectures.

## Decision

mcplay's core model is **client harness + mock replay**:

1. **Record**: mcplay connects to a real MCP server as an MCP **client** over stdio,
   drives it (interactively or via a script), and captures every JSON-RPC exchange into a
   portable **scenario file**.
2. **Replay**: mcplay acts as an MCP **server** over stdio, answering a downstream client's
   requests from a scenario file (a deterministic mock). No real server is required at
   replay time.

Re-driving a live server with recorded requests (option a) is provided in a narrower form
by the **scripted `mcplay run`** command (verification against a live server), but the
primary "replay" surface is the mock server.

## Rationale

- Mock replay is the most faithful reading of "record → replay": reproduce a server's
  behavior offline, for demos, deterministic tests, and sharing.
- It decouples downstream development from the availability/cost/nondeterminism of the real
  server.
- The scripted-run command covers the regression/assertion use case without making the
  whole product depend on a live server.

## Consequences

- mcplay must implement BOTH an MCP client (recorder) and an MCP server (mock). Both use
  `@modelcontextprotocol/sdk`.
- A **matching engine** is required to map an incoming replay request to a recorded
  interaction (see `docs/DESIGN.md` §Matching Engine). Match modes: `strict`,
  `sequential`, `best-effort`.
- Non-determinism in recorded servers (timestamps, random IDs) is handled by the matching
  engine's normalization rules and by redaction, not by re-execution.

## Alternatives considered

| Option | Why not as the primary model |
|---|---|
| Live re-drive only (regression) | Requires the real server at replay; loses the offline/sharing value. Kept as `mcplay run`. |
| Server authoring/hosting toolkit | Diverges from record/replay/share; different product. |
| Everything in v1 (client + mock + live re-drive, all capabilities) | Scope too large for v1; staged instead. |
