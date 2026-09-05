# mcplay — Design

Status: v1 design (authoritative)
Last updated: 2026-07-06
Owners: Human user (product) + Fable (architecture/design)

> This document is the canonical design source of truth for mcplay. GitHub Issues and Pull
> Requests are derived artifacts. If they disagree with this document, this document wins
> and the derived artifacts are stale.

---

## 1. Overview

mcplay is a **CLI-first, open-source tool for exercising Model Context Protocol (MCP)
servers**. It connects to an MCP server as a client over the `stdio` transport, lets a
developer drive it interactively or with a script, **records** every JSON-RPC exchange into
a portable **scenario file**, and can later **replay** that scenario as a **mock MCP
server** so the recorded behavior can be reproduced offline, shared, and used for
deterministic testing and demos.

One-line: *record what an MCP server does, replay it without the real server, share the
recording as a file.*

### Confirmed product decisions (see ADRs)

| Decision | Value | ADR |
|---|---|---|
| Language / runtime | TypeScript on Node.js 20+, npm distribution | ADR-001 |
| Form factor | CLI-first (web UI is v2) | ADR-002 |
| Core model | Client harness (record) + mock-server replay | ADR-003 |
| Sharing | Local file I/O only, no backend | ADR-004 |
| Security | Explicit trust boundaries, secure defaults, fail-closed | ADR-005 |
| Scenario format | JSON `.mcplay.json`, zod schema, versioned | ADR-006 |
| Transport (v1) | `stdio` only (Streamable HTTP is v2) | ADR-003 |
| LLM in loop (v1) | None; manual + deterministic scripts (LLM-driven is v2) | this doc §12 |
| Target protocol | `2025-11-25` stable; version stored per scenario | research/mcp-protocol-grounding.md |
| MCP capability MVP | **tools** only; resources/prompts/sampling/roots/elicitation are separately-planned issues | this doc §7 |

---

## 2. Goals and non-goals

### 2.1 v1 goals

- G1. Spawn and connect to a local (stdio) MCP server safely.
- G2. Interactively list and call **tools** and record the exchanges.
- G3. Persist recordings as portable, versioned, human-diffable scenario files.
- G4. Replay a scenario as a mock MCP server (tools) that a downstream client can connect
  to over stdio, with deterministic responses.
- G5. Run a scripted scenario against a live server with assertions (`mcplay run`).
- G6. Inspect, validate, diff, redact, list, export, and import scenarios locally.
- G7. Enforce the security model (ADR-005) with secure defaults and fail-closed behavior.
- G8. Ship as an npm package with a hardened OSS release posture.

### 2.2 v1 non-goals

- N1. No Streamable HTTP / remote transport (v2).
- N2. No LLM-driven exploration or any LLM in the loop (v2).
- N3. No web/GUI (v2).
- N4. No hosted registry, sync server, accounts, or discovery (v2+).
- N5. No authoring/scaffolding of new MCP servers.
- N6. No live re-drive of arbitrary recorded sessions beyond the scripted `mcplay run`.

### 2.3 Capability staging (per human decision)

- **MVP path:** tools capability only (record + replay + script). This is the smallest
  usable product.
- **Planned extension issues (post-MVP, still in this plan):** resources, prompts,
  sampling, roots, elicitation, completions — **each as its own granular issue** for both
  record and replay. See `docs/ISSUE_PLAN.md` Wave 8.

---

## 3. Personas & primary user journeys

- **P1 MCP server author** — records their server's tool behavior to build a deterministic
  test fixture and to share a reproducible bug report.
- **P2 MCP client/integration developer** — replays a shared scenario as a mock so they can
  develop against a server they do not have installed.
- **P3 Reviewer/educator** — inspects and diffs scenarios to understand or teach how a
  server behaves.

### Journeys

- J1 Record: `mcplay record -- <server-cmd> [args]` → interactive REPL → call tools →
  `save` → `my-server.mcplay.json`.
- J2 Replay: `mcplay replay my-server.mcplay.json` → mcplay is now a stdio MCP server;
  point Claude Desktop / another client at it.
- J3 Verify: `mcplay run smoke.mcplay.script.json -- <server-cmd>` → runs steps, checks
  assertions, exits non-zero on failure (CI-friendly).
- J4 Share: `mcplay redact my-server.mcplay.json` → `mcplay export` → send file → recipient
  `mcplay import` + `mcplay validate`.

---

## 4. High-level architecture

```
                    +-------------------------------------------------+
                    |                     CLI (src/cli)               |
                    |  record | replay | run | inspect | validate |   |
                    |  redact | diff | ls | export | import | init    |
                    +------------------------+------------------------+
                                             | calls (UI-agnostic core)
             +-------------------------------v-------------------------------+
             |                          Core (src/core)                       |
             |                                                                |
             |  recorder/    replay/      script/     scenario/    redaction/ |
             |  (client      (mock        (runner +   (schema,     (rules)    |
             |   harness)     server)      asserts)    store,                 |
             |                                          migrate)              |
             |                                                                |
             |  mcp/client   mcp/server   mcp/process   security/   config/   |
             |  (SDK client) (SDK server) (spawn mgr)   (policy)    (loader)  |
             |                                                                |
             |  diff/        logging/     errors/       util/                 |
             +---------------------------+------------------------------------+
                                         |
                +------------------------v------------------------+
                |   @modelcontextprotocol/sdk  +  Node child_process |
                +----------------------------------------------------+
                                         |
                             stdio JSON-RPC to/from
                       real MCP server (record) OR downstream
                            client (replay/mock)
```

Design rule (from ADR-002): **all product logic lives in `src/core/**` and is UI-agnostic.**
`src/cli/**` is a thin adapter that parses args, calls core, and formats output. This keeps
a future web UI (v2) a pure addition.

---

## 5. Module & file layout

Directory layout implementation agents must follow. One issue generally maps to one module
directory or one CLI command file.

```
mcplay/
  package.json                # bin: { mcplay: dist/cli/index.js }, engines: node >=20
  tsconfig.json
  vitest.config.ts
  src/
    cli/
      index.ts                # arg parser + subcommand dispatch (commander)
      commands/
        record.ts
        replay.ts
        run.ts
        inspect.ts
        validate.ts
        redact.ts
        diff.ts
        ls.ts
        export.ts
        import.ts
        init.ts
      output.ts               # human/JSON formatting, exit-code mapping
      repl.ts                 # interactive record REPL
    core/
      mcp/
        client.ts             # MCP client wrapper (SDK Client + StdioClientTransport)
        server.ts             # mock MCP server wrapper (SDK low-level Server)
        process.ts            # secure child-process spawn + lifecycle
        types.ts              # shared MCP-facing types
      recorder/
        recorder.ts           # session recorder core (state machine)
        capture.ts            # interaction capture model + transport tap
        catalog.ts            # server catalog snapshot (initialize + *_list)
      replay/
        server-adapter.ts     # binds a scenario to the mock server handlers
        match.ts              # matching engine (strict|sequential|best-effort)
      script/
        schema.ts             # script (steps) zod schema
        runner.ts             # executes steps against a live server
        assert.ts             # assertion engine
      scenario/
        schema.ts             # scenario zod schema (SOURCE OF TRUTH) + types
        store.ts              # read/write, atomic, stable serialization
        migrate.ts            # schemaVersion migrations
        lint.ts               # security lint over a loaded scenario
      redaction/
        rules.ts              # default + custom redaction rules
        redactor.ts           # apply/report redaction
      security/
        command-policy.ts     # allowlist + embedded-command confirmation gate
        env.ts                # env allowlist resolution
        path.ts               # path validation / traversal & symlink guards
        limits.ts             # timeouts, output-size caps, depth caps
      diff/
        diff.ts               # scenario diff model + formatter
      config/
        config.ts             # config discovery, load, defaults, validation
      logging/
        logger.ts             # structured, level-controlled logging
      errors/
        errors.ts             # typed error taxonomy + exit-code mapping
      util/
        json.ts, canonical.ts, id.ts, time.ts
  test/
    fixtures/                 # sample servers + scenarios for tests
  docs/                       # this design set
```

---

## 6. CLI surface (v1)

Global conventions:
- Binary: `mcplay`. Help: `mcplay --help`, `mcplay <cmd> --help`.
- Global flags: `--config <path>`, `--log-level <error|warn|info|debug>`,
  `--json` (machine-readable output), `--no-color`, `--yes` (assume-yes for confirmations,
  MUST still refuse to auto-run embedded commands unless combined with the explicit
  `--allow-command` gate — see §11.2), `--version`.
- `--` separates mcplay flags from the target server command:
  `mcplay record -- python server.py --flag`. Everything after `--` is the server argv.

| Command | Purpose | Key flags |
|---|---|---|
| `record` | Spawn a server, drive it, capture a scenario | `-o/--out <file>`, `--script <file>`, `--env KEY[=VAL]`, `--cwd <dir>`, `--name <str>`, `--no-redact`, `--timeout-ms`, `--max-bytes` |
| `replay` | Serve a scenario as a mock MCP server (stdio) | `<scenario>`, `--match <strict\|sequential\|best-effort>`, `--strict-unmatched` |
| `run` | Run a script against a live server, assert | `<script>`, `-- <server-cmd>`, `-o/--out <file>` (also record), `--fail-fast` |
| `inspect` | Human summary of a scenario | `<scenario>`, `--json` |
| `validate` | Schema + security lint of a scenario/script | `<file>`, `--strict` |
| `redact` | Apply redaction rules to a scenario in place/out | `<scenario>`, `-o/--out`, `--rule <name>` |
| `diff` | Diff two scenarios | `<a> <b>`, `--json` |
| `ls` | List scenarios in the scenarios dir | `--dir <path>`, `--json` |
| `export` | Normalize + checksum + redaction gate for sharing | `<scenario>`, `-o/--out` |
| `import` | Validate + integrity-check an incoming scenario | `<file>`, `--dir <path>` |
| `init` | Scaffold a config file | `--global` |

Exit codes: `0` success; `1` generic failure; `2` usage error; `3` validation/lint failure;
`4` assertion failure (`run`); `5` security-policy refusal. Mapped in `src/core/errors`.

---

## 7. Data model

### 7.1 Scenario schema (v1, `schemaVersion: "1"`)

Defined in `src/core/scenario/schema.ts` with zod. Field reference (implementation agents
must match names exactly):

```jsonc
{
  "schemaVersion": "1",              // string; readers reject unknown majors w/o migration
  "mcplayVersion": "0.1.0",          // producer version
  "id": "uuid-v4",                   // stable scenario id
  "name": "example-fs-server",       // human label
  "description": "optional",         // optional
  "createdAt": "2026-07-06T00:00:00.000Z", // ISO-8601 UTC
  "target": {
    "transport": "stdio",            // v1: literal "stdio"
    "command": "python",             // executable name/path (DATA ONLY, untrusted)
    "args": ["server.py"],           // argv (DATA ONLY)
    "cwd": null,                     // optional recorded working dir (string|null)
    "envKeys": ["FOO_TOKEN"]         // NAMES ONLY; values never stored by default
  },
  "protocol": {
    "requestedVersion": "2025-11-25",
    "negotiatedVersion": "2025-11-25"
  },
  "server": {
    "name": "fs-server",
    "version": "1.2.3",
    "capabilities": { /* verbatim server capabilities object */ },
    "instructions": null             // optional server instructions string
  },
  "catalog": {                       // snapshots captured during recording
    "tools": [ /* Tool[] verbatim from tools/list */ ],
    "resources": null,               // populated only when resources capability recorded
    "resourceTemplates": null,
    "prompts": null                  // populated only when prompts capability recorded
  },
  "interactions": [ /* Interaction[] in capture order */ ],
  "redaction": {
    "applied": true,
    "rules": ["default-secrets", "env-values"]
  }
}
```

`Interaction`:

```jsonc
{
  "index": 0,                        // 0-based capture order
  "direction": "client->server",     // or "server->client"
  "method": "tools/call",            // JSON-RPC method
  "kind": "request-response",        // "request-response" | "notification"
  "request": {
    "id": 1,                         // JSON-RPC id (number|string|null)
    "params": { "name": "read_file", "arguments": { "path": "a.txt" } }
  },
  "response": {                      // present for request-response; null for notifications
    "result": { /* verbatim result */ },
    "error": null                    // or { code, message, data }
  },
  "notifications": [ /* server->client notifications observed during this call */ ],
  "timing": { "startedAt": "ISO-8601", "durationMs": 12 },
  "truncated": false,                // true if payload exceeded max-bytes and was capped
  "redacted": true
}
```

Normalization for matching (see §9): `canonical.ts` produces a stable canonical form of
`method` + `request.params` (sorted keys, normalized whitespace) used as the match key.

### 7.2 Script schema (v1, separate file `*.mcplay.script.json`)

Defined in `src/core/script/schema.ts`.

```jsonc
{
  "schemaVersion": "1",
  "name": "smoke",
  "steps": [
    { "action": "initialize" },
    { "action": "list_tools" },
    {
      "action": "call_tool",
      "name": "read_file",
      "arguments": { "path": "a.txt" },
      "expect": {                    // optional assertions
        "isError": false,
        "resultContains": "hello"    // substring/JSON-subset match (see assert.ts)
      }
    }
    // post-MVP actions (separate issues): read_resource, get_prompt, ...
  ]
}
```

---

## 8. Recorder design

### 8.1 Interception strategy

The SDK `Client` exposes high-level helpers but the recorder needs exact request/response
pairs and any interleaved notifications. Strategy: wrap the `StdioClientTransport` with a
**transport tap** (`capture.ts`) that observes `send` (outbound) and `onmessage` (inbound)
frames, correlating by JSON-RPC `id`, while the high-level `Client` still drives the
protocol. If the SDK version in use does not permit clean wrapping, fall back to composing
around the transport's public `send`/`onmessage`. (Known unknown — see research note.)

### 8.2 Record session state machine

```
idle ──start──▶ spawning ──spawned──▶ initializing ──initialized──▶ ready
   │                │                       │                          │
   │                └─spawn-error─▶ error    └─init-error─▶ error        │
   │                                                                    │
ready ──drive(tools/list, tools/call, ...)──▶ ready   (each captured)   │
ready ──save──▶ finalizing ──written──▶ closed                          │
ready ──server-exit/crash──▶ finalizing(incomplete) ──written──▶ closed │
any   ──fatal──▶ error ──cleanup(kill child)──▶ closed                  │
```

Rules:
- `initialize` and `notifications/initialized` are always captured first.
- The server catalog (`tools/list` for MVP) is snapshotted into `catalog` on entering
  `ready` and re-snapshotted on `notifications/tools/list_changed`.
- On crash, a partial scenario is still written with an `incomplete` marker in metadata; no
  partial file is written on spawn failure.
- Child process is always killed on exit paths (`SIGTERM` then `SIGKILL` after grace).

### 8.3 Interactive REPL (record)

`src/cli/repl.ts` commands (MVP):
- `tools` / `list` — list tools from catalog.
- `call <tool> <json-args>` — call a tool; result printed and captured.
- `raw <method> <json-params>` — send an arbitrary request (advanced; still captured).
- `redact` — toggle redaction preview.
- `save [file]` / `quit` / `help`.

Non-interactive mode: if `--script` is passed or stdin is not a TTY, the REPL is replaced by
the script runner feeding the same recorder.

---

## 9. Replay / mock server design

### 9.1 Mock server

`replay/server-adapter.ts` builds an SDK low-level `Server` over `StdioServerTransport`
whose handlers answer from the scenario:
- `initialize` → returns `server.name/version/capabilities` and negotiates protocol version
  (echo `negotiatedVersion`, or the client's requested version if compatible).
- `tools/list` → returns `catalog.tools`.
- `tools/call` → routed through the **matching engine** to a recorded interaction's
  response (result or error).
- Unknown/unmatched → behavior controlled by match mode (below).

### 9.2 Matching engine (`replay/match.ts`)

Input: an incoming request (method + params). Output: the recorded response to return.

| Mode | Behavior |
|---|---|
| `strict` (default) | Match on canonical (method + params). Exactly one recorded match required. No match → return a JSON-RPC error `-32001 mcplay/unmatched`. Multiple identical recordings → return in recorded order (stateful cursor). |
| `sequential` | Ignore params; return the next recorded interaction for that method in order. Useful for stateful sequences. |
| `best-effort` | Prefer exact canonical match; else fall back to same-method nearest by param similarity; annotate response as approximate. |

`--strict-unmatched` forces an error on any miss even in `best-effort`. All match decisions
are logged at `debug`.

### 9.3 Determinism

Recorded timestamps/ids in payloads are returned verbatim (the mock reproduces the
recording). The mock does NOT synthesize new nondeterministic values. Any per-call dynamic
fields a consumer needs are out of scope for v1 (documented limitation).

---

## 10. Scripted run & assertions

`script/runner.ts` executes steps sequentially against a live server (reusing the recorder
harness so a run can also record with `-o`). `script/assert.ts` evaluates `expect` clauses:
- `isError: boolean` — asserts the tool result error flag.
- `resultContains: string` — substring match against serialized text content.
- `resultMatches: object` — JSON-subset (deep partial) match against the result.
- `errorCode: number` — asserts a JSON-RPC error code.
Failures accumulate; `--fail-fast` stops at first. Exit code `4` on any assertion failure.

---

## 11. Security model (detailed) — see ADR-005

### 11.1 Trust boundaries & expectations

| # | Boundary | Threats | v1 controls (must-have) |
|---|---|---|---|
| B1 | Child process spawn | Arbitrary/malicious command execution, shell injection, env-secret leakage, fork bombs, hang | `shell:false` + argv array only; env allowlist (§11.3); cwd validation; per-request + session timeouts; output-size caps; process killed on all exits; no privilege escalation |
| B2 | Scenario/script file parse | Malicious shared file: huge/deeply-nested JSON (DoS), unexpected fields, path traversal via embedded paths, RCE via embedded command | zod strict schema; byte-size cap; JSON depth cap; reject unknown fields (`.strict()`); embedded `command` never auto-executed (§11.2); embedded paths validated (§11.4) |
| B3 | Secret handling | Secrets in tool args/results/env recorded then shared | Redaction on by default (§11.5); env values never stored; pre-export redaction gate; never log secret values |
| B4 | Replay output | Mock coerced into reading files / leaking host data | Mock serves ONLY in-memory scenario data; no filesystem reads driven by client input |
| B5 | Persistence | Unsafe writes, world-readable secrets, traversal on save/import | Atomic writes within validated base dir; restrictive file mode (0600) for scenarios that may contain secrets; traversal/symlink guards |

Out of scope v1 (documented): network/SSRF (no HTTP), auth (no backend), multi-tenant.

### 11.2 Command policy & embedded-command gate (`security/command-policy.ts`)

- The target server command for `record`/`run` comes from the CLI argv after `--` — an
  explicit user action, allowed.
- A command **embedded inside a scenario/script file** is untrusted. `replay` never needs
  it (mock uses data only). Any flow that would execute a file-embedded command MUST require
  an explicit `--allow-command` flag AND an interactive confirmation (unless `--yes` is
  combined with `--allow-command`; even then it is logged prominently). Default: refuse
  (exit code 5).
- No shell is ever invoked. Commands are resolved and spawned via `child_process.spawn`
  with `shell:false`.

### 11.3 Environment allowlist (`security/env.ts`)

- Default spawn env is a minimal safe base (`PATH`, `HOME`, `TMPDIR`, locale) — NOT the full
  parent env.
- Additional vars require explicit `--env KEY` (inherit value from parent) or
  `--env KEY=VALUE`. Only the **names** of passed vars may be stored in `target.envKeys`;
  values are never persisted.

### 11.4 Path validation (`security/path.ts`)

- All input/output file paths (scenarios, scripts, cwd, config) are resolved and checked to
  remain within an allowed base (scenarios dir / explicit `--out` / CWD as appropriate).
- Reject `..` traversal and symlinks that escape the base. Reject writing outside the
  intended directory.

### 11.5 Redaction (`redaction/**`)

- Default rules (`default-secrets`): high-entropy tokens, `Authorization` values, common key
  patterns (`*_token`, `*_secret`, `*_key`, `password`, `apiKey`, AWS/GCP key shapes).
- `env-values` rule: never persist env values (enforced structurally, not just by regex).
- Redaction is ON by default during recording; `--no-redact` opt-out prints a prominent
  warning and marks the scenario `redaction.applied=false`.
- `export` runs a redaction gate: if a scenario is not marked redacted, warn and require
  confirmation before producing the shareable artifact.

### 11.6 Limits (`security/limits.ts`)

- `--timeout-ms` per request (default 30000) and a session timeout.
- `--max-bytes` per captured payload (default 1 MiB); oversize payloads truncated with
  `truncated:true`.
- JSON parse depth cap (default 64) and total file-size cap (default 16 MiB) on load.

### 11.7 Supply chain / release

- Minimal, pinned dependencies; committed lockfile; `npm ci` in CI.
- GitHub Actions hardened: least-privilege `permissions`, pinned action SHAs, no untrusted
  PR secrets. Dependabot + CodeQL + secret scanning + push protection enabled.
- `SECURITY.md` with disclosure policy; provenance on publish.

---

## 12. LLM-in-the-loop (explicitly none in v1)

v1 has **no LLM** driving anything. Recording is manual (REPL) or scripted (deterministic
steps). This removes provider abstraction, API-key storage, and cost controls from the v1
attack surface. LLM-driven exploration (e.g., Claude selecting and calling tools to
auto-generate scenarios) is a **v2** feature and would reintroduce a credential boundary; it
must not be partially implemented in v1.

---

## 13. Configuration

`config/config.ts` discovers config in this order (first found wins), merged over defaults:
1. `--config <path>`
2. `./mcplay.config.json` (project)
3. `$XDG_CONFIG_HOME/mcplay/config.json` or `~/.config/mcplay/config.json` (global)

Fields:
```jsonc
{
  "scenariosDir": "./scenarios",
  "defaultMatchMode": "strict",
  "redaction": { "enabled": true, "rules": ["default-secrets", "env-values"] },
  "limits": { "requestTimeoutMs": 30000, "maxPayloadBytes": 1048576, "maxFileBytes": 16777216, "jsonDepth": 64 },
  "logLevel": "info"
}
```
Config values are validated by zod; invalid config fails closed (exit 3).

---

## 14. Failure modes (must be handled)

| Scenario | Expected behavior |
|---|---|
| Server binary not found (ENOENT) | Clear error, exit 1, no file written |
| Server crashes mid-session | Kill, write partial scenario w/ `incomplete`, exit 1 |
| Server hangs on a request | Per-request timeout → error captured, session continues or aborts |
| Malformed JSON-RPC from server | Capture raw frame flagged; do not crash |
| Oversized payload | Truncate + `truncated:true`; warn |
| Unknown `schemaVersion` on load | Refuse unless migration exists (exit 3) |
| Replay request with no match (strict) | JSON-RPC error `-32001 mcplay/unmatched` |
| Scenario file with embedded command on replay | Never executed; mock uses data only |
| Path traversal in input path | Refuse (exit 5) |
| Corrupt/oversized scenario file on load | Refuse with validation error (exit 3) |

---

## 15. Testing strategy (whole product)

- **Framework:** vitest. Coverage target ≥ 80% on `src/core/**`.
- **Unit:** schema validation, matching engine (all modes), redaction rules, path/env/limit
  guards, canonicalization, migration.
- **Integration:** a bundled minimal reference MCP server fixture (stdio) under
  `test/fixtures/servers/` used to drive real record → replay round-trips.
- **Round-trip property:** record against the fixture, then replay, then a client re-issues
  the same calls and receives identical results (golden comparison).
- **Security tests (must exist):** shell-injection attempt is inert; env not inherited;
  embedded-command refusal; oversized/deeply-nested file rejected; traversal rejected;
  redaction removes seeded secrets; `--no-redact` warns and marks file.
- **CLI tests:** each command's happy path + exit codes.
- **CI:** typecheck + lint + unit + integration on Node 20 and 22.

---

## 16. Scope summary

### 16.1 v1 (MVP) — must complete for a usable product
Waves 0–7 in `docs/ISSUE_PLAN.md`: scaffolding, security/process foundations, scenario
schema/store, tools client harness, recorder (tools), replay mock (tools), scripted run,
inspect/diff/redact/ls/export/import/init, docs + security acceptance + release.

### 16.2 v1 planned extensions (still issued, staged after MVP)
Wave 8: resources, prompts, sampling, roots, elicitation, completions — each as separate
record and replay issues.

### 16.3 v2 (deferred, not issued as v1 tasks)
Streamable HTTP transport; local web UI; LLM-driven exploration; self-hostable sync server;
single-binary distribution; scenario discovery/search.

### 16.4 Known unknowns (may create issues during implementation)
- SDK hook for raw transport frame capture (recorder tap).
- Whether low-level `Server` fully supports arbitrary recorded schemas for the mock.
- Canonicalization edge cases for param matching (ordering of arrays, floating point).
- Redaction false-negative/positive tuning against real-world servers.

---

## 17. Coverage map

Section-to-issue coverage is maintained in `docs/ISSUE_PLAN.md` §Coverage Table. Any design
section not covered by at least one issue is a planning defect and must be fixed there.
