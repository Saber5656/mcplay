# mcplay — Issue Plan

Status: authoritative issue plan (derived GitHub Issues must match this)
Last updated: 2026-07-06

> This plan is derived from `docs/DESIGN.md` and the ADRs. GitHub Issues are derived from
> the `docs/issues/NN-*.md` drafts. If they disagree, update the docs first.

---

## 1. v1 completion statement

The v1 product is complete when:

- **MVP (Waves 0–7)** is done: mcplay can safely spawn a stdio MCP server, interactively or
  by script record its **tools** behavior into a versioned `.mcplay.json` scenario, replay
  that scenario as a deterministic mock MCP server, run scripted assertions against a live
  server, and inspect/validate/diff/redact/list/export/import scenarios — all under the
  security model in ADR-005.
- **Release & acceptance (Wave 9)** is done: security acceptance tests, round-trip
  integration tests, user docs, `SECURITY.md`/threat model, OSS hardening, and the npm
  publish workflow all pass.
- **Capability extensions (Wave 8)** are *planned and issued* per the human decision that
  MVP is tools-only while every other MCP capability (resources, prompts, sampling, roots,
  elicitation, completions) is a separate granular issue. Wave 8 completion upgrades mcplay
  from tools-only MVP to full-capability v1. A shippable tools-only release is possible
  after Wave 7 + Wave 9; Wave 8 may ship incrementally as v1.1…v1.x.

If every issue below is completed and validated, the intended v1 product exists, except for
the known unknowns in §7 that may spawn follow-up issues.

---

## 2. Issue list (recommended execution order)

| # | File | Title | Wave |
|---|---|---|---|
| 01 | `docs/issues/01-project-scaffolding.md` | Project scaffolding & build toolchain | 0 |
| 02 | `docs/issues/02-error-taxonomy.md` | Error taxonomy & exit-code mapping | 0 |
| 03 | `docs/issues/03-logging-module.md` | Structured logging module | 0 |
| 04 | `docs/issues/04-test-harness-fixture-server.md` | Test harness + reference MCP server fixture | 0 |
| 05 | `docs/issues/05-ci-workflow.md` | Hardened CI workflow | 0 |
| 06 | `docs/issues/06-secure-process-spawner.md` | Secure child-process spawner | 1 |
| 07 | `docs/issues/07-env-allowlist.md` | Environment allowlist module | 1 |
| 08 | `docs/issues/08-path-validation.md` | Path validation module | 1 |
| 09 | `docs/issues/09-limits.md` | Limits (timeouts / size / depth) module | 1 |
| 10 | `docs/issues/10-command-policy.md` | Command policy & embedded-command gate | 1 |
| 11 | `docs/issues/11-scenario-schema.md` | Scenario zod schema & types | 2 |
| 12 | `docs/issues/12-canonicalization.md` | Canonicalization utility | 2 |
| 13 | `docs/issues/13-scenario-store.md` | Scenario store (read/write/atomic/serialize) | 2 |
| 14 | `docs/issues/14-scenario-migration.md` | Scenario migration framework | 2 |
| 15 | `docs/issues/15-scenario-lint.md` | Scenario security lint | 2 |
| 16 | `docs/issues/16-config-loader.md` | Config loader | 2 |
| 17 | `docs/issues/17-mcp-client-wrapper.md` | MCP client wrapper (connect/init/tools) | 3 |
| 18 | `docs/issues/18-transport-tap-capture.md` | Transport tap & interaction capture model | 3 |
| 19 | `docs/issues/19-recorder-core.md` | Recorder core & session state machine | 4 |
| 20 | `docs/issues/20-catalog-snapshot.md` | Catalog snapshot (tools) | 4 |
| 21 | `docs/issues/21-redaction-engine.md` | Redaction engine (rules + redactor) | 4 |
| 22 | `docs/issues/22-record-command.md` | `mcplay record` command + REPL | 4 |
| 23 | `docs/issues/23-matching-engine.md` | Replay matching engine | 5 |
| 24 | `docs/issues/24-mock-server-adapter.md` | Mock server adapter (tools) | 5 |
| 25 | `docs/issues/25-replay-command.md` | `mcplay replay` command | 5 |
| 26 | `docs/issues/26-script-schema.md` | Script schema | 6 |
| 27 | `docs/issues/27-assertion-engine.md` | Assertion engine | 6 |
| 28 | `docs/issues/28-script-runner.md` | Script runner | 6 |
| 29 | `docs/issues/29-run-command.md` | `mcplay run` command | 6 |
| 30 | `docs/issues/30-validate-command.md` | `mcplay validate` command | 7 |
| 31 | `docs/issues/31-inspect-command.md` | `mcplay inspect` command | 7 |
| 32 | `docs/issues/32-ls-command.md` | `mcplay ls` command | 7 |
| 33 | `docs/issues/33-diff-command.md` | Scenario diff engine + `mcplay diff` | 7 |
| 34 | `docs/issues/34-redact-command.md` | `mcplay redact` command | 7 |
| 35 | `docs/issues/35-export-command.md` | `mcplay export` command | 7 |
| 36 | `docs/issues/36-import-command.md` | `mcplay import` command | 7 |
| 37 | `docs/issues/37-init-command.md` | `mcplay init` command | 7 |
| 38 | `docs/issues/38-resources-record.md` | Resources: client ops + record | 8 |
| 39 | `docs/issues/39-resources-replay.md` | Resources: replay/mock | 8 |
| 40 | `docs/issues/40-prompts-record.md` | Prompts: client ops + record | 8 |
| 41 | `docs/issues/41-prompts-replay.md` | Prompts: replay/mock | 8 |
| 42 | `docs/issues/42-sampling.md` | Sampling (server→client) record + mock | 8 |
| 43 | `docs/issues/43-roots.md` | Roots (client capability) record + mock | 8 |
| 44 | `docs/issues/44-elicitation.md` | Elicitation (server→client) record + mock | 8 |
| 45 | `docs/issues/45-completions.md` | Completions record + mock | 8 |
| 46 | `docs/issues/46-script-actions-extended.md` | Script actions & assertions for resources/prompts | 8 |
| 47 | `docs/issues/47-security-acceptance-tests.md` | Security acceptance test suite | 9 |
| 48 | `docs/issues/48-roundtrip-integration-tests.md` | Round-trip integration test suite | 9 |
| 49 | `docs/issues/49-user-docs.md` | User documentation | 9 |
| 50 | `docs/issues/50-security-md-threat-model.md` | SECURITY.md + threat model | 9 |
| 51 | `docs/issues/51-oss-hardening.md` | OSS repository hardening config | 9 |
| 52 | `docs/issues/52-release-workflow.md` | npm release/publish workflow + provenance | 9 |

---

## 3. Dependency table

Each issue lists prerequisites. Cross-cutting foundation issues (01–10) are prerequisites
for most later work.

| # | Depends on |
|---|---|
| 01 | — |
| 02 | 01 |
| 03 | 01 |
| 04 | 01 |
| 05 | 01 |
| 06 | 01, 02, 03, 09 |
| 07 | 01, 02 |
| 08 | 01, 02 |
| 09 | 01, 02 |
| 10 | 01, 02, 08 |
| 11 | 01 |
| 12 | 01 |
| 13 | 01, 08, 11 |
| 14 | 11 |
| 15 | 11, 10 |
| 16 | 01, 08, 11 |
| 17 | 06, 07, 11 |
| 18 | 11, 17 |
| 19 | 17, 18, 20, 21 |
| 20 | 11, 17 |
| 21 | 11 |
| 22 | 13, 16, 19, 21, 10 |
| 23 | 11, 12 |
| 24 | 11, 13, 23 |
| 25 | 13, 16, 24 |
| 26 | 11 |
| 27 | 11, 12 |
| 28 | 17, 19, 26, 27 |
| 29 | 13, 16, 28 |
| 30 | 11, 15, 16, 26 |
| 31 | 11, 13 |
| 32 | 13, 16 |
| 33 | 11, 12, 13 |
| 34 | 13, 21 |
| 35 | 13, 21, 15 |
| 36 | 13, 15 |
| 37 | 16 |
| 38 | 17, 19, 11 |
| 39 | 24, 23 |
| 40 | 17, 19, 11 |
| 41 | 24, 23 |
| 42 | 17, 18, 24 |
| 43 | 17, 24 |
| 44 | 17, 18, 24 |
| 45 | 17, 24 |
| 46 | 26, 27, 28, 38, 40 |
| 47 | 06, 07, 08, 09, 10, 15, 21 |
| 48 | 22, 25, 29, 04 |
| 49 | 22, 25, 29, 30–37 |
| 50 | ADR-005 |
| 51 | 01, 05 |
| 52 | 01, 05, 51 |

Critical path (MVP): 01 → (06,08,09) → 11 → 13 → 17 → 18 → 19 → 22 → 23 → 24 → 25 → 48.

---

## 4. Implementation waves

| Wave | Theme | Issues | Gate to next wave |
|---|---|---|---|
| 0 | Foundations | 01–05 | Build, lint, typecheck, and a test run green in CI |
| 1 | Security & process | 06–10 | Security primitives unit-tested; fail-closed verified |
| 2 | Scenario schema & storage | 11–16 | Schema round-trips; store atomic; lint rejects bad files |
| 3 | Client harness (tools) | 17–18 | Connect + initialize + tools list/call against fixture server; frames captured |
| 4 | Recorder (tools MVP) | 19–22 | `mcplay record` produces a valid scenario from the fixture |
| 5 | Replay mock (tools MVP) | 23–25 | `mcplay replay` serves the scenario; downstream client gets recorded results |
| 6 | Scripted run | 26–29 | `mcplay run` executes a script with assertions and correct exit codes |
| 7 | Utility & sharing | 30–37 | All utility commands work; export/import round-trips with integrity |
| 8 | Capability extensions | 38–46 | Each capability records + replays; extends MVP to full capability |
| 9 | Release & acceptance | 47–52 | Security + round-trip suites pass; docs complete; publishable |

A tools-only public release is possible after Waves 0–7 + Wave 9. Wave 8 ships
incrementally.

---

## 5. Coverage table (DESIGN.md section → issues)

| DESIGN.md section | Covered by issues |
|---|---|
| §4 Architecture / §5 Module layout | 01 (skeleton dirs) |
| §6 CLI surface | 22, 25, 29, 30, 31, 32, 33, 34, 35, 36, 37 (+ 02 exit codes) |
| §7.1 Scenario schema | 11, 12, 13, 14 |
| §7.2 Script schema | 26 |
| §8 Recorder | 17, 18, 19, 20, 22 |
| §9 Replay / matching | 23, 24, 25 |
| §10 Scripted run & assertions | 27, 28, 29, 46 |
| §11.1 Trust boundaries | 06, 07, 08, 09, 10, 15, 47, 50 |
| §11.2 Command policy | 10 |
| §11.3 Env allowlist | 07 |
| §11.4 Path validation | 08 |
| §11.5 Redaction | 21, 34, 35 |
| §11.6 Limits | 09 |
| §11.7 Supply chain / release | 05, 51, 52 |
| §12 No LLM in v1 | (constraint; no code issue) |
| §13 Configuration | 16, 37 |
| §14 Failure modes | 06, 09, 13, 14, 19, 23, 24 (+ tests 47, 48) |
| §15 Testing strategy | 04, 47, 48 |
| §16.2 Capability extensions | 38–46 |
| §17 Coverage | this table |

Every implementation-relevant DESIGN.md section maps to ≥1 issue. §12 is a constraint
(no LLM), enforced by absence, and reviewed in 47/50.

---

## 6. Whole-product validation strategy

1. **Static:** TypeScript strict typecheck + ESLint + Prettier must pass (Issue 01, 05).
2. **Unit:** every `src/core/**` module ships unit tests; ≥80% coverage (Issue 04 harness).
3. **Security acceptance:** a dedicated suite proves each secure default (Issue 47) — no
   shell exec, env not inherited, embedded-command refusal, oversized/deep-JSON rejection,
   path traversal refusal, redaction efficacy, `--no-redact` warning.
4. **Round-trip integration:** record→replay→re-call golden equality against the fixture
   server (Issue 48).
5. **CLI contract:** each command's exit codes and `--json` output are tested.
6. **CI matrix:** Node 20 + 22 (Issue 05).
7. **Release gate:** publish workflow runs the full suite before `npm publish` (Issue 52).

---

## 7. Deferred v2 items (NOT issued as v1 tasks)

- Streamable HTTP transport (adds network/SSRF boundary — new security issues required).
- Local web UI dashboard.
- LLM-driven exploration (reintroduces credential boundary + provider abstraction).
- Self-hostable sync server; hosted registry/discovery; accounts.
- Single-binary distribution (Node SEA / Bun / pkg).
- Scenario search / dedup / library management.

---

## 8. Known unknowns (may create additional issues during implementation)

- KU1: SDK hook for raw transport-frame capture; may require a thin custom transport
  (affects Issue 18).
- KU2: Whether the low-level SDK `Server` fully supports arbitrary recorded tool schemas
  for the mock (affects Issue 24).
- KU3: Canonicalization edge cases (array ordering, floats, unicode) for matching
  (affects Issue 12, 23).
- KU4: Redaction rule tuning against real servers (affects Issue 21) — may add rules.
- KU5: Partial-recording semantics on server crash (affects Issue 19) — may need a
  dedicated resumability issue.
- KU6: Forward-compat with MCP `2026-07-28` once final (may add a protocol-version issue).
