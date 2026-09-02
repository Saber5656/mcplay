# Wave 8 concrete review-resolution correction v3

- Repository: `Saber5656/mcplay`
- Pull request: #1
- Current PR head identity: the authoritative value is supplied by the immutable review manifest; this file intentionally omits the mutable commit SHA to avoid a self-referential identity.
- Current PR base pinned for this review: `d8a05d37e3cbd58940af65a22b4c654e56e35781`
- Previous correction artifact blob SHA: `ede7280a03fb9f791c8e84b51d541456e7ee77ea`
- This v3 artifact supersedes the earlier generic resolution addenda for the exact threads below.
- The immutable review manifest pins the current head, current base, and correction-file blob identity for this review; any later change invalidates this evidence and requires a fresh review.
- This is documentation-level handling for documentation-only PRs. It is not a claim that product implementation, runtime tests, build, CI, security, or release validation is complete.
- No PR review bot is re-triggered. The file's own current blob SHA is intentionally held only in the immutable review manifest because embedding it here would be self-referential.

## Per-repository blocking handoffs

| Repository | QA/full-validation handoff | Security/privacy handoff |
|---|---|---|
| `Saber5656/earmark#1` | `tech-qa`/`tech-tester` must execute API/schema, codec, TTS, lifecycle, launchd, queue, notification, docs-lint, and repository-full-validation gates for this head/base. | `tech-security`/`tech-devopssec` must accept credential URL, remote egress, temp-file, output, path, static-audit, and packaging boundaries. |
| `Saber5656/tracegit#1` | `tech-qa`/`tech-tester` must execute record/schema, parser, CLI, editor, note, sync, E2E, docs-lint, and repository-full-validation gates for this head/base. | `tech-security`/`tech-devopssec` must accept note-size, file-mode, ref-update, sanitization, output-bound, and Git execution boundaries. |
| `Saber5656/mcplay#1` | `tech-qa`/`tech-tester` must execute schema, recorder, replay, dependency, CLI, protocol, docs-lint, and repository-full-validation gates for this head/base. | `tech-security`/`tech-devopssec` must accept spawn, path, permission, redaction, embedded-command, export, persistence, and threat-model boundaries. |

These handoffs are blocking: missing, pending, failed, skipped, cancelled, timed-out, stale, or non-accepting specialist/full-validation evidence prevents thread resolution and merge.

## Thread contracts

### 1. Thread `PRRT_kwDOTN39s86OdfiK` — Define how exported checksums fit the scenario format

- File: `docs/issues/35-export-command.md`
- Line: 17
- Finding basis: This export step requires adding a checksum to the shareable artifact, but Issue 11 makes the scenario schema strict and DESIGN §7.1 has no checksum field. If export embeds the checksum in the .mcplay.json , import/validate will reject it as an unknown field; if it emits a sidecar, mcplay import <file has no specified way to receive that sidecar. Please define either a schema field or a concrete bundle/sidecar interface before requiring import to verify the checksum.

**Normative resolution**: At `docs/issues/35-export-command.md:17`, the canonical contract SHALL adopt this exact decision: Export SHALL emit a sibling `<scenario>.mcplay.json.sha256` sidecar for the primary JSON; import SHALL require and verify that sidecar for exported bundles before schema processing, with missing/malformed/mismatched data failing closed.

**Focused verification gate**: Export/import a matching sidecar, then remove, corrupt, mismatch, and alter the primary file; assert only the unchanged matching pair imports.

**Completion boundary**: This contract is design-level only. Resolve the GitHub thread only after the focused gate, applicable specialist handoff, and repository full validation are terminal success for the current head/base identity.

### 2. Thread `PRRT_kwDOTN39s86OdfiM` — Keep issue 13 prerequisites in the dependency table

- File: `docs/ISSUE_PLAN.md`
- Line: 112
- Finding basis: This row omits Issue 09 and Issue 14 even though docs/issues/13-scenario-store.md requires size/depth limiting and migration before loadScenario is complete. If the authoritative dependency table is used to generate or schedule the work, the scenario store can be implemented before its required security limits and migration path exist, leading to skipped checks or placeholder unknown-version handling. Add the missing prerequisites here to keep the plan executable.

**Normative resolution**: At `docs/ISSUE_PLAN.md:112`, the canonical contract SHALL add Issues 09 and 14 as prerequisites of Issue 13, and scheduling Issue 13 SHALL be blocked until both are complete.

**Focused verification gate**: Build the ISSUE_PLAN graph and assert edges `09→13` and `14→13`; reject any order that schedules 13 first.

**Completion boundary**: This contract is design-level only. Resolve the GitHub thread only after the focused gate, applicable specialist handoff, and repository full validation are terminal success for the current head/base identity.

### 3. Thread `PRRT_kwDOTN39s86OdfiN` — Specify a valid transport path for the secure spawner

- File: `docs/issues/17-mcp-client-wrapper.md`
- Line: 16
- Finding basis: This requires Issue 06 to spawn the child and then connect the SDK client to those existing stdio streams, but the design/research elsewhere names StdioClientTransport({ command, args, env, cwd }) , which owns the subprocess spawn rather than wrapping already-open streams. Without explicitly requiring a custom stream transport (or another SDK API that accepts streams), implementers will either bypass the centralized secure spawner or be blocked by an API shape that does not match this issue.

**Normative resolution**: At `docs/issues/17-mcp-client-wrapper.md:16`, the canonical contract SHALL adopt this exact decision: Issue 17 SHALL use a custom stream-backed SDK transport over the streams already owned by Issue 06; it SHALL not pass command arguments to a transport that spawns a second child.

**Focused verification gate**: Connect a client to pre-opened streams and the real spawner; assert one child, correct framing, and no SDK-owned second spawn.

**Completion boundary**: This contract is design-level only. Resolve the GitHub thread only after the focused gate, applicable specialist handoff, and repository full validation are terminal success for the current head/base identity.

### 4. Thread `PRRT_kwDOTN39s86OdfiO` — Add schema support for incomplete recordings

- File: `docs/issues/19-recorder-core.md`
- Line: 18
- Finding basis: This asks crash handling to return a valid scenario with an incomplete marker in metadata, but the strict scenario schema in DESIGN §7.1/Issue 11 has no metadata object or incomplete field. In the server-crash path, the recorder therefore cannot both satisfy this requirement and produce a schema-valid file; define the marker in the schema before requiring crash recordings to be valid.

**Normative resolution**: At `docs/issues/19-recorder-core.md:18`, the canonical contract SHALL adopt this exact decision: The strict schema SHALL add `metadata.incomplete: boolean`, default false for complete recordings and true only for crash/interrupted recordings; crash persistence SHALL remain schema-valid.

**Focused verification gate**: Validate complete, clean-stop, crash-partial, missing, wrong-type, and unknown-field scenarios; assert only crash-partial uses `metadata.incomplete=true`.

**Completion boundary**: This contract is design-level only. Resolve the GitHub thread only after the focused gate, applicable specialist handoff, and repository full validation are terminal success for the current head/base identity.

### 5. Thread `PRRT_kwDOTN39s86OdfiP` — Don't schedule record --script before scripts exist

- File: `docs/issues/22-record-command.md`
- Line: 21
- Finding basis: This makes record --script part of Issue 22, but the script schema/runner it depends on are Issues 26 and 28 in a later wave and are not dependencies here. If issues are executed in the documented order, the record command must either invent a temporary script implementation or ship the advertised non-interactive mode incomplete; add the script issues as prerequisites or split record --script into the script wave.

**Normative resolution**: At `docs/issues/22-record-command.md:21`, the canonical contract SHALL adopt this exact decision: `record --script` SHALL be blocked until Issues 26 and 28 are complete; it must not invent a temporary script implementation.

**Focused verification gate**: Evaluate the issue graph with Issues 26/28 absent and present; assert `record --script` is blocked first and enabled only after both.

**Completion boundary**: This contract is design-level only. Resolve the GitHub thread only after the focused gate, applicable specialist handoff, and repository full validation are terminal success for the current head/base identity.

### 6. Thread `PRRT_kwDOTN39s86OdfiR` — Handle protocol pings in the replay mock

- File: `docs/issues/24-mock-server-adapter.md`
- Line: 18
- Finding basis: The mock adapter only registers initialize , tools/list , and tools/call , so a downstream MCP client that sends the core ping request after connecting will fall into the unmatched path instead of receiving the required empty result. The research note already marks ping as MVP/pass-through, so add an explicit ping handler and test here to avoid healthy clients treating the mock as stale.

**Normative resolution**: At `docs/issues/24-mock-server-adapter.md:18`, the canonical contract SHALL register the MCP `ping` request and return an empty successful result; a ping integration test SHALL fail if the request reaches the unmatched path.

**Focused verification gate**: Send `ping` before/after initialize and between calls; assert empty success, no interaction consumption, and protocol error for malformed params.

**Completion boundary**: This contract is design-level only. Resolve the GitHub thread only after the focused gate, applicable specialist handoff, and repository full validation are terminal success for the current head/base identity.

### 7. Thread `PRRT_kwDOTN39s86OdfiT` — Preserve replay matching across redacted request values

- File: `docs/issues/21-redaction-engine.md`
- Line: 22
- Finding basis: When a recorded tool argument contains a secret-like field such as apiKey or password , the default redaction pass replaces that request value with a placeholder, while strict replay matching uses the canonical recorded method+params. A downstream client replaying the same call with the original or a new secret will not match the placeholder and will get mcplay/unmatched ; define redacted values as wildcards or store a safe match key before making redaction on by default.

**Normative resolution**: At `docs/issues/21-redaction-engine.md:22`, the canonical contract SHALL adopt this exact decision: Redacted fields SHALL be represented by typed markers in the match key and act as wildcards only at marked paths; method, structure, and non-redacted fields remain strict and raw secrets are never stored.

**Focused verification gate**: Replay redacted secret fields with original/new values and changed non-secret/method/shape; assert only marked secret paths wildcard and no secret is stored.

**Completion boundary**: This contract is design-level only. Resolve the GitHub thread only after the focused gate, applicable specialist handoff, and repository full validation are terminal success for the current head/base identity.

### 8. Thread `PRRT_kwDOTN39s86OdfiW` — Allow protocol payloads to keep extension fields

- File: `docs/issues/11-scenario-schema.md`
- Line: 28
- Finding basis: Rejecting unknown nested fields is unsafe for the verbatim MCP payloads stored under server.capabilities , catalog.tools , and interaction results/errors: any valid tool metadata, meta , or future protocol extension field that the recorder captures can make the whole scenario fail validation. Keep strictness for mcplay-owned envelopes, but make protocol-owned payload objects passthrough/JSON values so valid recordings remain loadable.

**Normative resolution**: At `docs/issues/11-scenario-schema.md:28`, the canonical contract SHALL adopt this exact decision: mcplay-owned envelopes remain strict, while protocol-owned payload objects retain unknown extension keys as JSON values subject to size/depth limits.

**Focused verification gate**: Validate extension keys under capabilities/tools/results plus malformed owned envelopes and depth/size limits; assert extensions round-trip and envelope violations fail.

**Completion boundary**: This contract is design-level only. Resolve the GitHub thread only after the focused gate, applicable specialist handoff, and repository full validation are terminal success for the current head/base identity.

### 9. Thread `PRRT_kwDOTN39s86OdfiX` — Filter unsupported capabilities from mock initialize

- File: `docs/issues/24-mock-server-adapter.md`
- Line: 17
- Finding basis: Returning the recorded server capabilities verbatim breaks tools-only replay for real servers that also advertised resources, prompts, sampling, or other non-MVP capabilities: the mock tells downstream clients those features exist even though this issue only implements tool handlers and explicitly defers resources/prompts. Filter initialize capabilities to the handlers actually backed by the scenario, or clients can legitimately call advertised methods and immediately receive unmatched errors.

**Normative resolution**: At `docs/issues/24-mock-server-adapter.md:17`, the canonical contract SHALL adopt this exact decision: The tools-only mock SHALL advertise only the capabilities it actually handles, omitting resources, prompts, sampling, and other unsupported capabilities.

**Focused verification gate**: Initialize tools-only and mixed-capability scenarios and call advertised/omitted methods; assert only backed capabilities are advertised.

**Completion boundary**: This contract is design-level only. Resolve the GitHub thread only after the focused gate, applicable specialist handoff, and repository full validation are terminal success for the current head/base identity.

### 10. Thread `PRRT_kwDOTN39s86OdfiZ` — Replay roots as a server-initiated request

- File: `docs/issues/43-roots.md`
- Line: 17
- Finding basis: For roots, the recorded flow is the server issuing roots/list to the client (as the preceding recorder requirement states), and during replay mcplay is again the server. If the mock instead waits to answer a downstream roots/list call, the recorded roots exchange is never reproduced for clients that expect the server to request roots; make the mock initiate the recorded request and handle the response/list-changed notification direction correctly.

**Normative resolution**: At `docs/issues/43-roots.md:17`, the canonical contract SHALL adopt this exact decision: Roots replay SHALL preserve the recorded server-to-client `roots/list` direction, correlate the response, and handle `roots/list_changed`; it SHALL not wait for a client-initiated request when the recording is server-initiated.

**Focused verification gate**: Replay server-initiated roots/list with response and list-changed; assert direction, correlation, order, and roots values match.

**Completion boundary**: This contract is design-level only. Resolve the GitHub thread only after the focused gate, applicable specialist handoff, and repository full validation are terminal success for the current head/base identity.

### 11. Thread `PRRT_kwDOTN39s86Odf2T` — Add language tags to the fenced blocks

- File: `docs/DESIGN.md`
- Line: 127
- Finding basis: markdownlint is already flagging these bare fences. Tag them ( text , jsonc , etc.) so the docs stay lint-clean. Also applies to: 140-205, 351-360

**Normative resolution**: At `docs/DESIGN.md:127`, the canonical contract SHALL adopt this exact decision: Every fenced block in the listed DESIGN ranges SHALL have a syntax-appropriate language tag; an untagged block is a documentation-lint failure.

**Focused verification gate**: Enumerate fences in ranges 127, 140–205, and 351–360; assert each has a syntax tag and markdownlint reports no MD040.

**Completion boundary**: This contract is design-level only. Resolve the GitHub thread only after the focused gate, applicable specialist handoff, and repository full validation are terminal success for the current head/base identity.

### 12. Thread `PRRT_kwDOTN39s86Odf2V` — Document `--allow-command` in the CLI surface

- File: `docs/DESIGN.md`
- Line: 216
- Finding basis: §11.2 makes this flag part of the security gate for file-embedded commands, but §6 never lists it. That leaves the authoritative CLI contract incomplete and easy to implement incorrectly.

**Normative resolution**: At `docs/DESIGN.md:216`, the canonical contract SHALL adopt this exact decision: DESIGN §6 SHALL document `--allow-command`; embedded commands remain refused unless both the flag and confirmation are present, and the flag does not bypass other controls.

**Focused verification gate**: Run default, flag-only, confirmation-only, and flag-plus-confirmation embedded-command cases; assert only the last can execute.

**Completion boundary**: This contract is design-level only. Resolve the GitHub thread only after the focused gate, applicable specialist handoff, and repository full validation are terminal success for the current head/base identity.

### 13. Thread `PRRT_kwDOTN39s86Odf2W` — Define the `incomplete` marker in the schema contract

- File: `docs/DESIGN.md`
- Line: 367
- Finding basis: The crash path promises a partial scenario with incomplete , but §7.1 has no field for that state. That makes the documented recovery path impossible to validate or persist consistently. Also applies to: 533-534

**Normative resolution**: At `docs/DESIGN.md:367`, the canonical contract SHALL adopt this exact decision: DESIGN §7.1 and the crash examples SHALL define `metadata.incomplete` as a boolean with default false and crash-only true semantics.

**Focused verification gate**: Validate complete/crash examples and missing, non-boolean, and unknown-field variants; assert field location/default/transition agree.

**Completion boundary**: This contract is design-level only. Resolve the GitHub thread only after the focused gate, applicable specialist handoff, and repository full validation are terminal success for the current head/base identity.

### 14. Thread `PRRT_kwDOTN39s86Odf2Y` — Add the missing prerequisites to the MVP critical path

- File: `docs/ISSUE_PLAN.md`
- Line: 153
- Finding basis: This sequence skips 10, 16, 21, and 12, but those are prerequisites for 22/23 in the dependency table. As written, the “critical path” is misleading; either expand it or relabel it as a narrower happy path.

**Normative resolution**: At `docs/ISSUE_PLAN.md:153`, the canonical contract SHALL adopt this exact decision: The authoritative critical path SHALL include Issues 10, 16, 21, and 12 before their 22/23 consumers; a shorter list must be labeled non-authoritative.

**Focused verification gate**: Generate the ISSUE_PLAN graph and assert 10, 16, 21, and 12 precede their 22/23 consumers; fail a mislabeled critical path.

**Completion boundary**: This contract is design-level only. Resolve the GitHub thread only after the focused gate, applicable specialist handoff, and repository full validation are terminal success for the current head/base identity.

### 15. Thread `PRRT_kwDOTN39s86Odf2a` — Tighten the write contract before implementing atomic saves

- File: `docs/issues/13-scenario-store.md`
- Line: 18
- Finding basis: Please state that both the temporary file and the final destination are confined to the validated scenarios base directory and created with 0600 . Atomic rename alone does not prevent a permissive temp file or an absolute/traversal path from leaking shared scenario contents.

**Normative resolution**: At `docs/issues/13-scenario-store.md:18`, the canonical contract SHALL adopt this exact decision: Scenario-store temp and final paths SHALL be inside the validated base, reject traversal/absolute/symlink escapes, use mode 0600, and publish atomically only after validation.

**Focused verification gate**: Attempt absolute, traversal, symlink, outside-base, collision, and valid scenario writes; inspect 0600 modes and assert no escape/partial file.

**Completion boundary**: This contract is design-level only. Resolve the GitHub thread only after the focused gate, applicable specialist handoff, and repository full validation are terminal success for the current head/base identity.

### 16. Thread `PRRT_kwDOTN39s86Odf2b` — Keep config discovery single-source, not merged

- File: `docs/issues/16-config-loader.md`
- Line: 18
- Finding basis: The design says config files are discovered in priority order and the first existing file wins; only that file is merged over defaults. Line 18 turns this into a merge across global/project/ --config , which will silently blend settings from multiple locations and break the documented precedence contract.

**Normative resolution**: At `docs/issues/16-config-loader.md:18`, the canonical contract SHALL adopt this exact decision: Config discovery SHALL choose the first existing source in documented priority order and merge only that source over defaults; it SHALL not merge global/project/explicit sources together.

**Focused verification gate**: Create conflicting global/project/explicit configs; assert first-existing selection, defaults-only merge, and selected-path diagnostics.

**Completion boundary**: This contract is design-level only. Resolve the GitHub thread only after the focused gate, applicable specialist handoff, and repository full validation are terminal success for the current head/base identity.

### 17. Thread `PRRT_kwDOTN39s86Odf2c` — Add Issue 09 to the dependency list

- File: `docs/issues/17-mcp-client-wrapper.md`
- Line: 34
- Finding basis: The detailed requirements pull per-request timeout handling from Issue 09, but the dependency list only names 06/07/11. That leaves the wrapper spec under-constrained.

**Normative resolution**: At `docs/issues/17-mcp-client-wrapper.md:34`, the canonical contract SHALL include Issue 09, and readiness SHALL fail when the Issue 09 timeout contract is absent.

**Focused verification gate**: Parse Issue 17 dependencies and a negative graph; assert `09→17` is explicit and missing 09 blocks readiness.

**Completion boundary**: This contract is design-level only. Resolve the GitHub thread only after the focused gate, applicable specialist handoff, and repository full validation are terminal success for the current head/base identity.

### 18. Thread `PRRT_kwDOTN39s86Odf2e` — Include Issue 09 in the recorder dependencies

- File: `docs/issues/19-recorder-core.md`
- Line: 35
- Finding basis: Line 24 depends on the timeout contract from Issue 09, but the dependency set stops at 17/18/20/21. Add 09 so the recorder state machine cannot be scheduled before its timeout semantics exist.

**Normative resolution**: At `docs/issues/19-recorder-core.md:35`, the canonical contract SHALL include Issue 09, and recorder scheduling SHALL be blocked until the Issue 09 timeout contract is complete.

**Focused verification gate**: Parse Issue 19 dependencies and a negative graph; assert `09→19` is explicit and missing 09 blocks the recorder.

**Completion boundary**: This contract is design-level only. Resolve the GitHub thread only after the focused gate, applicable specialist handoff, and repository full validation are terminal success for the current head/base identity.

### 19. Thread `PRRT_kwDOTN39s86Odf2g` — Add Issue 07 to the dependency list

- File: `docs/issues/21-redaction-engine.md`
- Line: 35
- Finding basis: env-values is explicitly structural and depends on the env allowlist work from Issue 07, but the Dependencies block only names 11. As written, this can be scheduled before its prerequisite exists.

**Normative resolution**: At `docs/issues/21-redaction-engine.md:35`, the canonical contract SHALL include Issue 07, and readiness SHALL fail when the Issue 07 environment allowlist contract is absent.

**Focused verification gate**: Parse Issue 21 dependencies and disable Issue 07 in a negative graph; assert `07→21` is explicit and readiness fails without it.

**Completion boundary**: This contract is design-level only. Resolve the GitHub thread only after the focused gate, applicable specialist handoff, and repository full validation are terminal success for the current head/base identity.

### 20. Thread `PRRT_kwDOTN39s86Odf2l` — Pin the best-effort tie-break

- File: `docs/issues/23-matching-engine.md`
- Line: 25
- Finding basis: best-effort still leaves equal-similarity candidates undefined, so replay can vary across runs. Specify a stable fallback, such as recorded order.

**Normative resolution**: At `docs/issues/23-matching-engine.md:25`, the canonical contract SHALL adopt this exact decision: Best-effort matching SHALL tie-break equal similarity by stable recorded order/index and mark approximate matches; strict mode errors on any miss.

**Focused verification gate**: Replay equal-similarity candidates repeatedly and in reordered inputs; assert the stable recorded index wins, approximate is flagged, and scenario data is unchanged.

**Completion boundary**: This contract is design-level only. Resolve the GitHub thread only after the focused gate, applicable specialist handoff, and repository full validation are terminal success for the current head/base identity.

### 21. Thread `PRRT_kwDOTN39s86Odf2m` — Add Issue 26 to the run-command dependencies

- File: `docs/issues/29-run-command.md`
- Line: 31
- Finding basis: The command scope explicitly says it must load and validate scripts via Issue 26, but the dependency list skips it. That hides a hard prerequisite and makes the command spec look implementable before the parser/validator exists.

**Normative resolution**: At `docs/issues/29-run-command.md:31`, the canonical contract SHALL include Issue 26, and run-command scheduling SHALL be blocked until the script parser/validator contract is complete.

**Focused verification gate**: Build the run-command graph with Issue 26 absent/present and run valid/invalid scripts; assert no execution occurs without the validator.

**Completion boundary**: This contract is design-level only. Resolve the GitHub thread only after the focused gate, applicable specialist handoff, and repository full validation are terminal success for the current head/base identity.

### 22. Thread `PRRT_kwDOTN39s86Odf2n` — Use `Interaction.index` in the diff key

- File: `docs/issues/33-diff-command.md`
- Line: 16
- Finding basis: method+params alone will collapse repeated identical calls, so diffScenarios should distinguish each occurrence with a stable per-interaction index (or equivalent tie-breaker) to avoid missing changes.

**Normative resolution**: At `docs/issues/33-diff-command.md:16`, the canonical contract SHALL adopt this exact decision: `diffScenarios` SHALL key by method, canonical params, and stable `Interaction.index`, using the same key for pairing and reporting.

**Focused verification gate**: Diff scenarios where only the second of repeated identical calls changes; assert the report names the second `Interaction.index`.

**Completion boundary**: This contract is design-level only. Resolve the GitHub thread only after the focused gate, applicable specialist handoff, and repository full validation are terminal success for the current head/base identity.

### 23. Thread `PRRT_kwDOTN39s86Odf2p` — Keep the export gate tied to `redaction.applied`

- File: `docs/issues/35-export-command.md`
- Line: 21
- Finding basis: docs/DESIGN.md:471-479 says export should warn and require confirmation when a scenario is not marked redacted. Making lint findings an outright blocker here changes that contract and can reject already-redacted files on false positives; keep lint in validation and gate export on the redaction flag.

**Normative resolution**: At `docs/issues/35-export-command.md:21`, the canonical contract SHALL adopt this exact decision: Export SHALL gate only on `redaction.applied`: false warns and requires confirmation, true may proceed; lint findings remain validation output rather than an independent blocker.

**Focused verification gate**: Export redacted/unredacted cases with clean/secret-like lint and confirmation combinations; assert only unredacted/no-confirmation blocks.

**Completion boundary**: This contract is design-level only. Resolve the GitHub thread only after the focused gate, applicable specialist handoff, and repository full validation are terminal success for the current head/base identity.

### 24. Thread `PRRT_kwDOTN39s86Odf2r` — Add the missing upstream dependencies

- File: `docs/issues/40-prompts-record.md`
- Line: 25
- Finding basis: This spec depends on both Issue 18 for notifications/prompts/list changed capture and Issue 21 for redaction of prompt payloads, but neither is listed. Without them, this work can be scheduled before the notification/redaction plumbing exists.

**Normative resolution**: At `docs/issues/40-prompts-record.md:25`, the canonical contract SHALL include Issues 18 and 21, and readiness SHALL fail when either notification-capture or redaction contract is absent.

**Focused verification gate**: Build the prompts-record graph and inspect notification/redaction fixtures; assert `18→40` and `21→40` edges and blocking when either is absent.

**Completion boundary**: This contract is design-level only. Resolve the GitHub thread only after the focused gate, applicable specialist handoff, and repository full validation are terminal success for the current head/base identity.

### 25. Thread `PRRT_kwDOTN39s86Odf2s` — Add Issue 40 as an explicit dependency

- File: `docs/issues/41-prompts-replay.md`
- Line: 26
- Finding basis: This replay spec consumes the prompts catalog shape and recorded prompt interactions introduced by the recorder work in Issue 40. Keeping that dependency implicit makes the replay task look implementable before the data model exists.

**Normative resolution**: At `docs/issues/41-prompts-replay.md:26`, the canonical contract SHALL include Issue 40, and replay scheduling SHALL be blocked until the recorder data model contract is complete.

**Focused verification gate**: Parse replay dependencies and topologically order them; assert `40→41` and blocking before recorder data exists.

**Completion boundary**: This contract is design-level only. Resolve the GitHub thread only after the focused gate, applicable specialist handoff, and repository full validation are terminal success for the current head/base identity.

### 26. Thread `PRRT_kwDOTN39s86Odf2u` — Capture transport notifications in the dependency graph

- File: `docs/issues/43-roots.md`
- Line: 30
- Finding basis: The scope includes roots/list changed notifications, but the dependency list only names Issues 17 and 24. Issue 18 is the piece that captures bidirectional notifications, so this work can be picked up before that plumbing exists.

**Normative resolution**: At `docs/issues/43-roots.md:30`, the canonical contract SHALL include Issue 18, and roots-notification scheduling SHALL be blocked until bidirectional notification capture is complete.

**Focused verification gate**: Inspect roots dependency and notification case; assert `18→43` and blocking without notification capture.

**Completion boundary**: This contract is design-level only. Resolve the GitHub thread only after the focused gate, applicable specialist handoff, and repository full validation are terminal success for the current head/base identity.

### 27. Thread `PRRT_kwDOTN39s86Odf2w` — Add Issue 21 to the prerequisites

- File: `docs/issues/44-elicitation.md`
- Line: 30
- Finding basis: This spec explicitly says elicited values are subject to redaction, but the dependency list stops at 17/18/24. Add the redaction engine issue so this work is not scheduled before the no-secret-leak path exists.

**Normative resolution**: At `docs/issues/44-elicitation.md:30`, the canonical contract SHALL include Issue 21, and elicitation scheduling SHALL be blocked until the redaction-engine contract is complete.

**Focused verification gate**: Inspect elicitation dependency and secret-value fixture; assert `21→44` and redaction before persistence/replay.

**Completion boundary**: This contract is design-level only. Resolve the GitHub thread only after the focused gate, applicable specialist handoff, and repository full validation are terminal success for the current head/base identity.

### 28. Thread `PRRT_kwDOTN39s86Odf2x` — Add coverage for the remaining B1 spawn controls

- File: `docs/issues/47-security-acceptance-tests.md`
- Line: 24
- Finding basis: This suite covers shell injection and env leakage, but DESIGN §11/B1 also requires cwd validation, per-request/session timeouts, output-size caps, and kill-on-exit. Without explicit tests for those controls, the security gate can still pass while the spawn boundary regresses.

**Normative resolution**: At `docs/issues/47-security-acceptance-tests.md:24`, the canonical contract SHALL adopt this exact decision: The B1 suite SHALL separately test cwd validation, per-request/session timeouts, output-size caps, kill-on-exit, shell inertness, env isolation, and the existing path/command/redaction/export controls.

**Focused verification gate**: Run invalid cwd, timeout, oversized stdout, process exit, shell metacharacter, secret env, embedded command, traversal/symlink, and unredacted export fixtures; assert each control stops at its boundary.

**Completion boundary**: This contract is design-level only. Resolve the GitHub thread only after the focused gate, applicable specialist handoff, and repository full validation are terminal success for the current head/base identity.

### 29. Thread `PRRT_kwDOTN39s86Odf2z` — Call out the replay and persistence controls explicitly

- File: `docs/issues/50-security-md-threat-model.md`
- Line: 18
- Finding basis: The scope says the threat model will cover B1–B5, but it only names generic limitations. DESIGN §11 also requires the replay boundary to stay in-memory/FS-isolated and the persistence boundary to use atomic, restricted writes; those controls need to be spelled out so the threat model matches the security contract.

**Normative resolution**: At `docs/issues/50-security-md-threat-model.md:18`, the canonical contract SHALL adopt this exact decision: The threat model SHALL map B4 replay isolation to in-memory/FS-isolated handling and B5 persistence safety to validated base paths, traversal/symlink guards, atomic writes, and restrictive modes.

**Focused verification gate**: Cross-check THREAT_MODEL.md, DESIGN §11, and security tests; assert B4/B5 each have threat, control, test reference, and residual limitation.

**Completion boundary**: This contract is design-level only. Resolve the GitHub thread only after the focused gate, applicable specialist handoff, and repository full validation are terminal success for the current head/base identity.

## Merge boundary

- `gate-task-evaluator` must re-fetch PR state, current head/base, required-check inventory, review decision, unresolved thread state, policy version, and merge candidate immediately before any merge mutation.
- `github_mergeable` or a successful CodeRabbit status is not merge authorization.
- The current task instruction allows at most one PR Bot review; this artifact authorizes no Bot trigger or rerun.