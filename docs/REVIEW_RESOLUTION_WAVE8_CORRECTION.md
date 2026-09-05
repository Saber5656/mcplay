# Wave 8 concrete review-resolution correction

- Repository: `Saber5656/mcplay`
- Pull request: #1
- Base SHA pinned before correction: `d8a05d37e3cbd58940af65a22b4c654e56e35781`
- PR head pinned as correction parent: `17cc2206dbfa3df9b2bf3518ac44c92b048952a9`
- This file is the concrete correction artifact for the existing PR and supersedes the generic wording in `docs/REVIEW_RESOLUTION.md` for these threads.
- Every entry is bound to the exact current review-thread ID, path, and line. Any later head/base/thread change invalidates the evidence and requires a fresh preflight.
- This is documentation-level contract handling for documentation-only PRs. It does not claim product implementation, runtime tests, build, CI, security, or release validation is complete.
- Focused verification, applicable QA/security review, repository full validation, and the separate merge gate remain blocking before resolve/merge.
- The PR review bot is not re-triggered.

## Gate contract

| Gate | Blocking rule |
|---|---|
| Thread identity | ID/path/line still match a current non-outdated thread |
| Normative contract | The SHALL statement below is the canonical decision |
| Focused verification | The per-thread check has terminal evidence |
| QA/security | Applicable specialist review accepts |
| Full validation | Repository-prescribed validation passes for final head |
| Merge | Current head/base/check/review/thread/policy identity passes |

## 1. Thread `PRRT_kwDOTN39s86OdfiK` — Define how exported checksums fit the scenario format

- File: `docs/issues/35-export-command.md`
- Line: 17
- Finding basis: This export step requires adding a checksum to the shareable artifact, but Issue 11 makes the scenario schema strict and DESIGN §7.1 has no checksum field. If export embeds the checksum in the .mcplay.json , import/validate will reject it as an unknown field; if it emits a sidecar, mcplay import <file has no specified way to receive that sidecar. Please define either a schema field or a concrete bundle/sidecar interface before requiring import to verify the checksum.

**Normative resolution**: At `docs/issues/35-export-command.md:17`, the canonical contract SHALL adopt the concrete requirement in this finding: This export step requires adding a checksum to the shareable artifact, but Issue 11 makes the scenario schema strict and DESIGN §7.1 has no checksum field. If export embeds the checksum in the .mcplay.json , import/validate will reject it as an unknown field; if it emits a sidecar, mcplay import <file has no specified way to receive that sidecar. Please define either a schema field or a concrete bundle/sidecar interface before requiring import to verify the checksum.. Missing, malformed, or mismatched integrity data blocks import/publication before side effects.

**Focused verification gate**: Validate canonical positive, missing, wrong-type, unknown-field, and boundary fixtures for `docs/issues/35-export-command.md:17`; assert the stated shape and failure behavior exactly.

**Completion boundary**: This entry records the design-level acceptance contract only. Do not resolve the GitHub thread until the focused result, applicable specialist review, and final repository validation are terminal success for the pinned identity.

## 2. Thread `PRRT_kwDOTN39s86OdfiM` — Keep issue 13 prerequisites in the dependency table

- File: `docs/ISSUE_PLAN.md`
- Line: 112
- Finding basis: This row omits Issue 09 and Issue 14 even though docs/issues/13-scenario-store.md requires size/depth limiting and migration before loadScenario is complete. If the authoritative dependency table is used to generate or schedule the work, the scenario store can be implemented before its required security limits and migration path exist, leading to skipped checks or placeholder unknown-version handling. Add the missing prerequisites here to keep the plan executable.

**Normative resolution**: At `docs/ISSUE_PLAN.md:112`, the canonical contract SHALL adopt the concrete requirement in this finding: This row omits Issue 09 and Issue 14 even though docs/issues/13-scenario-store.md requires size/depth limiting and migration before loadScenario is complete. If the authoritative dependency table is used to generate or schedule the work, the scenario store can be implemented before its required security limits and migration path exist, leading to skipped checks or placeholder unknown-version handling. Add the missing prerequisites here to keep the plan executable.. The named graph edge/exception is mandatory; a missing edge blocks scheduling and acceptance.

**Focused verification gate**: Build the dependency/layering graph from `docs/ISSUE_PLAN.md:112`; assert the named edge or exception is present, ordered correctly, and a negative graph is rejected.

**Completion boundary**: This entry records the design-level acceptance contract only. Do not resolve the GitHub thread until the focused result, applicable specialist review, and final repository validation are terminal success for the pinned identity.

## 3. Thread `PRRT_kwDOTN39s86OdfiN` — Specify a valid transport path for the secure spawner

- File: `docs/issues/17-mcp-client-wrapper.md`
- Line: 16
- Finding basis: This requires Issue 06 to spawn the child and then connect the SDK client to those existing stdio streams, but the design/research elsewhere names StdioClientTransport({ command, args, env, cwd }) , which owns the subprocess spawn rather than wrapping already-open streams. Without explicitly requiring a custom stream transport (or another SDK API that accepts streams), implementers will either bypass the centralized secure spawner or be blocked by an API shape that does not match this issue.

**Normative resolution**: At `docs/issues/17-mcp-client-wrapper.md:16`, the canonical contract SHALL adopt the concrete requirement in this finding: This requires Issue 06 to spawn the child and then connect the SDK client to those existing stdio streams, but the design/research elsewhere names StdioClientTransport({ command, args, env, cwd }) , which owns the subprocess spawn rather than wrapping already-open streams. Without explicitly requiring a custom stream transport (or another SDK API that accepts streams), implementers will either bypass the centralized secure spawner or be blocked by an API shape that does not match this issue.. Unsafe, malformed, or unsupported input must fail at the stated boundary without leaking data or creating an unintended side effect.

**Focused verification gate**: Run a safe control and each unsafe/boundary fixture named in the finding for `docs/issues/17-mcp-client-wrapper.md:16`; assert rejection/containment, no sensitive-data exposure, and no partial side effect.

**Completion boundary**: This entry records the design-level acceptance contract only. Do not resolve the GitHub thread until the focused result, applicable specialist review, and final repository validation are terminal success for the pinned identity.

## 4. Thread `PRRT_kwDOTN39s86OdfiO` — Add schema support for incomplete recordings

- File: `docs/issues/19-recorder-core.md`
- Line: 18
- Finding basis: This asks crash handling to return a valid scenario with an incomplete marker in metadata, but the strict scenario schema in DESIGN §7.1/Issue 11 has no metadata object or incomplete field. In the server-crash path, the recorder therefore cannot both satisfy this requirement and produce a schema-valid file; define the marker in the schema before requiring crash recordings to be valid.

**Normative resolution**: At `docs/issues/19-recorder-core.md:18`, the canonical contract SHALL adopt the concrete requirement in this finding: This asks crash handling to return a valid scenario with an incomplete marker in metadata, but the strict scenario schema in DESIGN §7.1/Issue 11 has no metadata object or incomplete field. In the server-crash path, the recorder therefore cannot both satisfy this requirement and produce a schema-valid file; define the marker in the schema before requiring crash recordings to be valid.. The positive and invalid shapes must be deterministic and compatible with downstream consumers.

**Focused verification gate**: Validate canonical positive, missing, wrong-type, unknown-field, and boundary fixtures for `docs/issues/19-recorder-core.md:18`; assert the stated shape and failure behavior exactly.

**Completion boundary**: This entry records the design-level acceptance contract only. Do not resolve the GitHub thread until the focused result, applicable specialist review, and final repository validation are terminal success for the pinned identity.

## 5. Thread `PRRT_kwDOTN39s86OdfiP` — Don't schedule record --script before scripts exist

- File: `docs/issues/22-record-command.md`
- Line: 21
- Finding basis: This makes record --script part of Issue 22, but the script schema/runner it depends on are Issues 26 and 28 in a later wave and are not dependencies here. If issues are executed in the documented order, the record command must either invent a temporary script implementation or ship the advertised non-interactive mode incomplete; add the script issues as prerequisites or split record --script into the script wave.

**Normative resolution**: At `docs/issues/22-record-command.md:21`, the canonical contract SHALL adopt the concrete requirement in this finding: This makes record --script part of Issue 22, but the script schema/runner it depends on are Issues 26 and 28 in a later wave and are not dependencies here. If issues are executed in the documented order, the record command must either invent a temporary script implementation or ship the advertised non-interactive mode incomplete; add the script issues as prerequisites or split record --script into the script wave.. Unsafe, malformed, or unsupported input must fail at the stated boundary without leaking data or creating an unintended side effect.

**Focused verification gate**: Run the positive control and the exact negative/boundary case named in the finding for `docs/issues/22-record-command.md:21`; assert the canonical result and error behavior.

**Completion boundary**: This entry records the design-level acceptance contract only. Do not resolve the GitHub thread until the focused result, applicable specialist review, and final repository validation are terminal success for the pinned identity.

## 6. Thread `PRRT_kwDOTN39s86OdfiR` — Handle protocol pings in the replay mock

- File: `docs/issues/24-mock-server-adapter.md`
- Line: 18
- Finding basis: The mock adapter only registers initialize , tools/list , and tools/call , so a downstream MCP client that sends the core ping request after connecting will fall into the unmatched path instead of receiving the required empty result. The research note already marks ping as MVP/pass-through, so add an explicit ping handler and test here to avoid healthy clients treating the mock as stale.

**Normative resolution**: At `docs/issues/24-mock-server-adapter.md:18`, the canonical contract SHALL adopt the concrete requirement in this finding: The mock adapter only registers initialize , tools/list , and tools/call , so a downstream MCP client that sends the core ping request after connecting will fall into the unmatched path instead of receiving the required empty result. The research note already marks ping as MVP/pass-through, so add an explicit ping handler and test here to avoid healthy clients treating the mock as stale.. Unsafe, malformed, or unsupported input must fail at the stated boundary without leaking data or creating an unintended side effect.

**Focused verification gate**: Run the positive control and the exact negative/boundary case named in the finding for `docs/issues/24-mock-server-adapter.md:18`; assert the canonical result and error behavior.

**Completion boundary**: This entry records the design-level acceptance contract only. Do not resolve the GitHub thread until the focused result, applicable specialist review, and final repository validation are terminal success for the pinned identity.

## 7. Thread `PRRT_kwDOTN39s86OdfiT` — Preserve replay matching across redacted request values

- File: `docs/issues/21-redaction-engine.md`
- Line: 22
- Finding basis: When a recorded tool argument contains a secret-like field such as apiKey or password , the default redaction pass replaces that request value with a placeholder, while strict replay matching uses the canonical recorded method+params. A downstream client replaying the same call with the original or a new secret will not match the placeholder and will get mcplay/unmatched ; define redacted values as wildcards or store a safe match key before making redaction on by default.

**Normative resolution**: At `docs/issues/21-redaction-engine.md:22`, the canonical contract SHALL adopt the concrete requirement in this finding: When a recorded tool argument contains a secret-like field such as apiKey or password , the default redaction pass replaces that request value with a placeholder, while strict replay matching uses the canonical recorded method+params. A downstream client replaying the same call with the original or a new secret will not match the placeholder and will get mcplay/unmatched ; define redacted values as wildcards or store a safe match key before making redaction on by default.. Unsafe, malformed, or unsupported input must fail at the stated boundary without leaking data or creating an unintended side effect.

**Focused verification gate**: Run a safe control and each unsafe/boundary fixture named in the finding for `docs/issues/21-redaction-engine.md:22`; assert rejection/containment, no sensitive-data exposure, and no partial side effect.

**Completion boundary**: This entry records the design-level acceptance contract only. Do not resolve the GitHub thread until the focused result, applicable specialist review, and final repository validation are terminal success for the pinned identity.

## 8. Thread `PRRT_kwDOTN39s86OdfiW` — Allow protocol payloads to keep extension fields

- File: `docs/issues/11-scenario-schema.md`
- Line: 28
- Finding basis: Rejecting unknown nested fields is unsafe for the verbatim MCP payloads stored under server.capabilities , catalog.tools , and interaction results/errors: any valid tool metadata, meta , or future protocol extension field that the recorder captures can make the whole scenario fail validation. Keep strictness for mcplay-owned envelopes, but make protocol-owned payload objects passthrough/JSON values so valid recordings remain loadable.

**Normative resolution**: At `docs/issues/11-scenario-schema.md:28`, the canonical contract SHALL adopt the concrete requirement in this finding: Rejecting unknown nested fields is unsafe for the verbatim MCP payloads stored under server.capabilities , catalog.tools , and interaction results/errors: any valid tool metadata, meta , or future protocol extension field that the recorder captures can make the whole scenario fail validation. Keep strictness for mcplay-owned envelopes, but make protocol-owned payload objects passthrough/JSON values so valid recordings remain loadable.. The positive and invalid shapes must be deterministic and compatible with downstream consumers.

**Focused verification gate**: Validate canonical positive, missing, wrong-type, unknown-field, and boundary fixtures for `docs/issues/11-scenario-schema.md:28`; assert the stated shape and failure behavior exactly.

**Completion boundary**: This entry records the design-level acceptance contract only. Do not resolve the GitHub thread until the focused result, applicable specialist review, and final repository validation are terminal success for the pinned identity.

## 9. Thread `PRRT_kwDOTN39s86OdfiX` — Filter unsupported capabilities from mock initialize

- File: `docs/issues/24-mock-server-adapter.md`
- Line: 17
- Finding basis: Returning the recorded server capabilities verbatim breaks tools-only replay for real servers that also advertised resources, prompts, sampling, or other non-MVP capabilities: the mock tells downstream clients those features exist even though this issue only implements tool handlers and explicitly defers resources/prompts. Filter initialize capabilities to the handlers actually backed by the scenario, or clients can legitimately call advertised methods and immediately receive unmatched errors.

**Normative resolution**: At `docs/issues/24-mock-server-adapter.md:17`, the canonical contract SHALL adopt the concrete requirement in this finding: Returning the recorded server capabilities verbatim breaks tools-only replay for real servers that also advertised resources, prompts, sampling, or other non-MVP capabilities: the mock tells downstream clients those features exist even though this issue only implements tool handlers and explicitly defers resources/prompts. Filter initialize capabilities to the handlers actually backed by the scenario, or clients can legitimately call advertised methods and immediately receive unmatched errors.. Unsafe, malformed, or unsupported input must fail at the stated boundary without leaking data or creating an unintended side effect.

**Focused verification gate**: Run the positive control and the exact negative/boundary case named in the finding for `docs/issues/24-mock-server-adapter.md:17`; assert the canonical result and error behavior.

**Completion boundary**: This entry records the design-level acceptance contract only. Do not resolve the GitHub thread until the focused result, applicable specialist review, and final repository validation are terminal success for the pinned identity.

## 10. Thread `PRRT_kwDOTN39s86OdfiZ` — Replay roots as a server-initiated request

- File: `docs/issues/43-roots.md`
- Line: 17
- Finding basis: For roots, the recorded flow is the server issuing roots/list to the client (as the preceding recorder requirement states), and during replay mcplay is again the server. If the mock instead waits to answer a downstream roots/list call, the recorded roots exchange is never reproduced for clients that expect the server to request roots; make the mock initiate the recorded request and handle the response/list-changed notification direction correctly.

**Normative resolution**: At `docs/issues/43-roots.md:17`, the canonical contract SHALL adopt the concrete requirement in this finding: For roots, the recorded flow is the server issuing roots/list to the client (as the preceding recorder requirement states), and during replay mcplay is again the server. If the mock instead waits to answer a downstream roots/list call, the recorded roots exchange is never reproduced for clients that expect the server to request roots; make the mock initiate the recorded request and handle the response/list-changed notification direction correctly.. Unsafe, malformed, or unsupported input must fail at the stated boundary without leaking data or creating an unintended side effect.

**Focused verification gate**: Run the positive control and the exact negative/boundary case named in the finding for `docs/issues/43-roots.md:17`; assert the canonical result and error behavior.

**Completion boundary**: This entry records the design-level acceptance contract only. Do not resolve the GitHub thread until the focused result, applicable specialist review, and final repository validation are terminal success for the pinned identity.

## 11. Thread `PRRT_kwDOTN39s86Odf2T` — Add language tags to the fenced blocks

- File: `docs/DESIGN.md`
- Line: 127
- Finding basis: markdownlint is already flagging these bare fences. Tag them ( text , jsonc , etc.) so the docs stay lint-clean. Also applies to: 140-205, 351-360

**Normative resolution**: At `docs/DESIGN.md:127`, the canonical contract SHALL adopt the concrete requirement in this finding: markdownlint is already flagging these bare fences. Tag them ( text , jsonc , etc.) so the docs stay lint-clean. Also applies to: 140-205, 351-360. The documentation/release gate must fail on the omitted or bypass form.

**Focused verification gate**: Run the stated lint/semantic audit on `docs/DESIGN.md:127` and the additional ranges/alias forms named in the finding; assert the omission or bypass fails.

**Completion boundary**: This entry records the design-level acceptance contract only. Do not resolve the GitHub thread until the focused result, applicable specialist review, and final repository validation are terminal success for the pinned identity.

## 12. Thread `PRRT_kwDOTN39s86Odf2V` — Document `--allow-command` in the CLI surface

- File: `docs/DESIGN.md`
- Line: 216
- Finding basis: §11.2 makes this flag part of the security gate for file-embedded commands, but §6 never lists it. That leaves the authoritative CLI contract incomplete and easy to implement incorrectly.

**Normative resolution**: At `docs/DESIGN.md:216`, the canonical contract SHALL adopt the concrete requirement in this finding: §11.2 makes this flag part of the security gate for file-embedded commands, but §6 never lists it. That leaves the authoritative CLI contract incomplete and easy to implement incorrectly.. Unsafe, malformed, or unsupported input must fail at the stated boundary without leaking data or creating an unintended side effect.

**Focused verification gate**: Run the positive control and the exact negative/boundary case named in the finding for `docs/DESIGN.md:216`; assert the canonical result and error behavior.

**Completion boundary**: This entry records the design-level acceptance contract only. Do not resolve the GitHub thread until the focused result, applicable specialist review, and final repository validation are terminal success for the pinned identity.

## 13. Thread `PRRT_kwDOTN39s86Odf2W` — Define the `incomplete` marker in the schema contract

- File: `docs/DESIGN.md`
- Line: 367
- Finding basis: The crash path promises a partial scenario with incomplete , but §7.1 has no field for that state. That makes the documented recovery path impossible to validate or persist consistently. Also applies to: 533-534

**Normative resolution**: At `docs/DESIGN.md:367`, the canonical contract SHALL adopt the concrete requirement in this finding: The crash path promises a partial scenario with incomplete , but §7.1 has no field for that state. That makes the documented recovery path impossible to validate or persist consistently. Also applies to: 533-534. The positive and invalid shapes must be deterministic and compatible with downstream consumers.

**Focused verification gate**: Validate canonical positive, missing, wrong-type, unknown-field, and boundary fixtures for `docs/DESIGN.md:367`; assert the stated shape and failure behavior exactly.

**Completion boundary**: This entry records the design-level acceptance contract only. Do not resolve the GitHub thread until the focused result, applicable specialist review, and final repository validation are terminal success for the pinned identity.

## 14. Thread `PRRT_kwDOTN39s86Odf2Y` — Add the missing prerequisites to the MVP critical path

- File: `docs/ISSUE_PLAN.md`
- Line: 153
- Finding basis: This sequence skips 10, 16, 21, and 12, but those are prerequisites for 22/23 in the dependency table. As written, the “critical path” is misleading; either expand it or relabel it as a narrower happy path.

**Normative resolution**: At `docs/ISSUE_PLAN.md:153`, the canonical contract SHALL adopt the concrete requirement in this finding: This sequence skips 10, 16, 21, and 12, but those are prerequisites for 22/23 in the dependency table. As written, the “critical path” is misleading; either expand it or relabel it as a narrower happy path.. The named graph edge/exception is mandatory; a missing edge blocks scheduling and acceptance.

**Focused verification gate**: Build the dependency/layering graph from `docs/ISSUE_PLAN.md:153`; assert the named edge or exception is present, ordered correctly, and a negative graph is rejected.

**Completion boundary**: This entry records the design-level acceptance contract only. Do not resolve the GitHub thread until the focused result, applicable specialist review, and final repository validation are terminal success for the pinned identity.

## 15. Thread `PRRT_kwDOTN39s86Odf2a` — Tighten the write contract before implementing atomic saves

- File: `docs/issues/13-scenario-store.md`
- Line: 18
- Finding basis: Please state that both the temporary file and the final destination are confined to the validated scenarios base directory and created with 0600 . Atomic rename alone does not prevent a permissive temp file or an absolute/traversal path from leaking shared scenario contents.

**Normative resolution**: At `docs/issues/13-scenario-store.md:18`, the canonical contract SHALL adopt the concrete requirement in this finding: Please state that both the temporary file and the final destination are confined to the validated scenarios base directory and created with 0600 . Atomic rename alone does not prevent a permissive temp file or an absolute/traversal path from leaking shared scenario contents.. Failure must be bounded, deterministic, and leave no partial or stale state.

**Focused verification gate**: Run success, failure, interruption, deadline, and collision fixtures for `docs/issues/13-scenario-store.md:18`; assert the bounded result, rollback/cleanup, and absence of partial state.

**Completion boundary**: This entry records the design-level acceptance contract only. Do not resolve the GitHub thread until the focused result, applicable specialist review, and final repository validation are terminal success for the pinned identity.

## 16. Thread `PRRT_kwDOTN39s86Odf2b` — Keep config discovery single-source, not merged

- File: `docs/issues/16-config-loader.md`
- Line: 18
- Finding basis: The design says config files are discovered in priority order and the first existing file wins; only that file is merged over defaults. Line 18 turns this into a merge across global/project/ --config , which will silently blend settings from multiple locations and break the documented precedence contract.

**Normative resolution**: At `docs/issues/16-config-loader.md:18`, the canonical contract SHALL adopt the concrete requirement in this finding: The design says config files are discovered in priority order and the first existing file wins; only that file is merged over defaults. Line 18 turns this into a merge across global/project/ --config , which will silently blend settings from multiple locations and break the documented precedence contract.. Unsafe, malformed, or unsupported input must fail at the stated boundary without leaking data or creating an unintended side effect.

**Focused verification gate**: Run the positive control and the exact negative/boundary case named in the finding for `docs/issues/16-config-loader.md:18`; assert the canonical result and error behavior.

**Completion boundary**: This entry records the design-level acceptance contract only. Do not resolve the GitHub thread until the focused result, applicable specialist review, and final repository validation are terminal success for the pinned identity.

## 17. Thread `PRRT_kwDOTN39s86Odf2c` — Add Issue 09 to the dependency list

- File: `docs/issues/17-mcp-client-wrapper.md`
- Line: 34
- Finding basis: The detailed requirements pull per-request timeout handling from Issue 09, but the dependency list only names 06/07/11. That leaves the wrapper spec under-constrained.

**Normative resolution**: At `docs/issues/17-mcp-client-wrapper.md:34`, the canonical contract SHALL adopt the concrete requirement in this finding: The detailed requirements pull per-request timeout handling from Issue 09, but the dependency list only names 06/07/11. That leaves the wrapper spec under-constrained.. The named graph edge/exception is mandatory; a missing edge blocks scheduling and acceptance.

**Focused verification gate**: Build the dependency/layering graph from `docs/issues/17-mcp-client-wrapper.md:34`; assert the named edge or exception is present, ordered correctly, and a negative graph is rejected.

**Completion boundary**: This entry records the design-level acceptance contract only. Do not resolve the GitHub thread until the focused result, applicable specialist review, and final repository validation are terminal success for the pinned identity.

## 18. Thread `PRRT_kwDOTN39s86Odf2e` — Include Issue 09 in the recorder dependencies

- File: `docs/issues/19-recorder-core.md`
- Line: 35
- Finding basis: Line 24 depends on the timeout contract from Issue 09, but the dependency set stops at 17/18/20/21. Add 09 so the recorder state machine cannot be scheduled before its timeout semantics exist.

**Normative resolution**: At `docs/issues/19-recorder-core.md:35`, the canonical contract SHALL adopt the concrete requirement in this finding: Line 24 depends on the timeout contract from Issue 09, but the dependency set stops at 17/18/20/21. Add 09 so the recorder state machine cannot be scheduled before its timeout semantics exist.. The named graph edge/exception is mandatory; a missing edge blocks scheduling and acceptance.

**Focused verification gate**: Build the dependency/layering graph from `docs/issues/19-recorder-core.md:35`; assert the named edge or exception is present, ordered correctly, and a negative graph is rejected.

**Completion boundary**: This entry records the design-level acceptance contract only. Do not resolve the GitHub thread until the focused result, applicable specialist review, and final repository validation are terminal success for the pinned identity.

## 19. Thread `PRRT_kwDOTN39s86Odf2g` — Add Issue 07 to the dependency list

- File: `docs/issues/21-redaction-engine.md`
- Line: 35
- Finding basis: env-values is explicitly structural and depends on the env allowlist work from Issue 07, but the Dependencies block only names 11. As written, this can be scheduled before its prerequisite exists.

**Normative resolution**: At `docs/issues/21-redaction-engine.md:35`, the canonical contract SHALL adopt the concrete requirement in this finding: env-values is explicitly structural and depends on the env allowlist work from Issue 07, but the Dependencies block only names 11. As written, this can be scheduled before its prerequisite exists.. The named graph edge/exception is mandatory; a missing edge blocks scheduling and acceptance.

**Focused verification gate**: Build the dependency/layering graph from `docs/issues/21-redaction-engine.md:35`; assert the named edge or exception is present, ordered correctly, and a negative graph is rejected.

**Completion boundary**: This entry records the design-level acceptance contract only. Do not resolve the GitHub thread until the focused result, applicable specialist review, and final repository validation are terminal success for the pinned identity.

## 20. Thread `PRRT_kwDOTN39s86Odf2l` — Pin the best-effort tie-break

- File: `docs/issues/23-matching-engine.md`
- Line: 25
- Finding basis: best-effort still leaves equal-similarity candidates undefined, so replay can vary across runs. Specify a stable fallback, such as recorded order.

**Normative resolution**: At `docs/issues/23-matching-engine.md:25`, the canonical contract SHALL adopt the concrete requirement in this finding: best-effort still leaves equal-similarity candidates undefined, so replay can vary across runs. Specify a stable fallback, such as recorded order.. Unsafe, malformed, or unsupported input must fail at the stated boundary without leaking data or creating an unintended side effect.

**Focused verification gate**: Run the positive control and the exact negative/boundary case named in the finding for `docs/issues/23-matching-engine.md:25`; assert the canonical result and error behavior.

**Completion boundary**: This entry records the design-level acceptance contract only. Do not resolve the GitHub thread until the focused result, applicable specialist review, and final repository validation are terminal success for the pinned identity.

## 21. Thread `PRRT_kwDOTN39s86Odf2m` — Add Issue 26 to the run-command dependencies

- File: `docs/issues/29-run-command.md`
- Line: 31
- Finding basis: The command scope explicitly says it must load and validate scripts via Issue 26, but the dependency list skips it. That hides a hard prerequisite and makes the command spec look implementable before the parser/validator exists.

**Normative resolution**: At `docs/issues/29-run-command.md:31`, the canonical contract SHALL adopt the concrete requirement in this finding: The command scope explicitly says it must load and validate scripts via Issue 26, but the dependency list skips it. That hides a hard prerequisite and makes the command spec look implementable before the parser/validator exists.. The named graph edge/exception is mandatory; a missing edge blocks scheduling and acceptance.

**Focused verification gate**: Build the dependency/layering graph from `docs/issues/29-run-command.md:31`; assert the named edge or exception is present, ordered correctly, and a negative graph is rejected.

**Completion boundary**: This entry records the design-level acceptance contract only. Do not resolve the GitHub thread until the focused result, applicable specialist review, and final repository validation are terminal success for the pinned identity.

## 22. Thread `PRRT_kwDOTN39s86Odf2n` — Use `Interaction.index` in the diff key

- File: `docs/issues/33-diff-command.md`
- Line: 16
- Finding basis: method+params alone will collapse repeated identical calls, so diffScenarios should distinguish each occurrence with a stable per-interaction index (or equivalent tie-breaker) to avoid missing changes.

**Normative resolution**: At `docs/issues/33-diff-command.md:16`, the canonical contract SHALL adopt the concrete requirement in this finding: method+params alone will collapse repeated identical calls, so diffScenarios should distinguish each occurrence with a stable per-interaction index (or equivalent tie-breaker) to avoid missing changes.. Unsafe, malformed, or unsupported input must fail at the stated boundary without leaking data or creating an unintended side effect.

**Focused verification gate**: Run the positive control and the exact negative/boundary case named in the finding for `docs/issues/33-diff-command.md:16`; assert the canonical result and error behavior.

**Completion boundary**: This entry records the design-level acceptance contract only. Do not resolve the GitHub thread until the focused result, applicable specialist review, and final repository validation are terminal success for the pinned identity.

## 23. Thread `PRRT_kwDOTN39s86Odf2p` — Keep the export gate tied to `redaction.applied`

- File: `docs/issues/35-export-command.md`
- Line: 21
- Finding basis: docs/DESIGN.md:471-479 says export should warn and require confirmation when a scenario is not marked redacted. Making lint findings an outright blocker here changes that contract and can reject already-redacted files on false positives; keep lint in validation and gate export on the redaction flag.

**Normative resolution**: At `docs/issues/35-export-command.md:21`, the canonical contract SHALL adopt the concrete requirement in this finding: docs/DESIGN.md:471-479 says export should warn and require confirmation when a scenario is not marked redacted. Making lint findings an outright blocker here changes that contract and can reject already-redacted files on false positives; keep lint in validation and gate export on the redaction flag.. Unsafe, malformed, or unsupported input must fail at the stated boundary without leaking data or creating an unintended side effect.

**Focused verification gate**: Run a safe control and each unsafe/boundary fixture named in the finding for `docs/issues/35-export-command.md:21`; assert rejection/containment, no sensitive-data exposure, and no partial side effect.

**Completion boundary**: This entry records the design-level acceptance contract only. Do not resolve the GitHub thread until the focused result, applicable specialist review, and final repository validation are terminal success for the pinned identity.

## 24. Thread `PRRT_kwDOTN39s86Odf2r` — Add the missing upstream dependencies

- File: `docs/issues/40-prompts-record.md`
- Line: 25
- Finding basis: This spec depends on both Issue 18 for notifications/prompts/list changed capture and Issue 21 for redaction of prompt payloads, but neither is listed. Without them, this work can be scheduled before the notification/redaction plumbing exists.

**Normative resolution**: At `docs/issues/40-prompts-record.md:25`, the canonical contract SHALL adopt the concrete requirement in this finding: This spec depends on both Issue 18 for notifications/prompts/list changed capture and Issue 21 for redaction of prompt payloads, but neither is listed. Without them, this work can be scheduled before the notification/redaction plumbing exists.. The named graph edge/exception is mandatory; a missing edge blocks scheduling and acceptance.

**Focused verification gate**: Build the dependency/layering graph from `docs/issues/40-prompts-record.md:25`; assert the named edge or exception is present, ordered correctly, and a negative graph is rejected.

**Completion boundary**: This entry records the design-level acceptance contract only. Do not resolve the GitHub thread until the focused result, applicable specialist review, and final repository validation are terminal success for the pinned identity.

## 25. Thread `PRRT_kwDOTN39s86Odf2s` — Add Issue 40 as an explicit dependency

- File: `docs/issues/41-prompts-replay.md`
- Line: 26
- Finding basis: This replay spec consumes the prompts catalog shape and recorded prompt interactions introduced by the recorder work in Issue 40. Keeping that dependency implicit makes the replay task look implementable before the data model exists.

**Normative resolution**: At `docs/issues/41-prompts-replay.md:26`, the canonical contract SHALL adopt the concrete requirement in this finding: This replay spec consumes the prompts catalog shape and recorded prompt interactions introduced by the recorder work in Issue 40. Keeping that dependency implicit makes the replay task look implementable before the data model exists.. The named graph edge/exception is mandatory; a missing edge blocks scheduling and acceptance.

**Focused verification gate**: Build the dependency/layering graph from `docs/issues/41-prompts-replay.md:26`; assert the named edge or exception is present, ordered correctly, and a negative graph is rejected.

**Completion boundary**: This entry records the design-level acceptance contract only. Do not resolve the GitHub thread until the focused result, applicable specialist review, and final repository validation are terminal success for the pinned identity.

## 26. Thread `PRRT_kwDOTN39s86Odf2u` — Capture transport notifications in the dependency graph

- File: `docs/issues/43-roots.md`
- Line: 30
- Finding basis: The scope includes roots/list changed notifications, but the dependency list only names Issues 17 and 24. Issue 18 is the piece that captures bidirectional notifications, so this work can be picked up before that plumbing exists.

**Normative resolution**: At `docs/issues/43-roots.md:30`, the canonical contract SHALL adopt the concrete requirement in this finding: The scope includes roots/list changed notifications, but the dependency list only names Issues 17 and 24. Issue 18 is the piece that captures bidirectional notifications, so this work can be picked up before that plumbing exists.. The named graph edge/exception is mandatory; a missing edge blocks scheduling and acceptance.

**Focused verification gate**: Build the dependency/layering graph from `docs/issues/43-roots.md:30`; assert the named edge or exception is present, ordered correctly, and a negative graph is rejected.

**Completion boundary**: This entry records the design-level acceptance contract only. Do not resolve the GitHub thread until the focused result, applicable specialist review, and final repository validation are terminal success for the pinned identity.

## 27. Thread `PRRT_kwDOTN39s86Odf2w` — Add Issue 21 to the prerequisites

- File: `docs/issues/44-elicitation.md`
- Line: 30
- Finding basis: This spec explicitly says elicited values are subject to redaction, but the dependency list stops at 17/18/24. Add the redaction engine issue so this work is not scheduled before the no-secret-leak path exists.

**Normative resolution**: At `docs/issues/44-elicitation.md:30`, the canonical contract SHALL adopt the concrete requirement in this finding: This spec explicitly says elicited values are subject to redaction, but the dependency list stops at 17/18/24. Add the redaction engine issue so this work is not scheduled before the no-secret-leak path exists.. The named graph edge/exception is mandatory; a missing edge blocks scheduling and acceptance.

**Focused verification gate**: Build the dependency/layering graph from `docs/issues/44-elicitation.md:30`; assert the named edge or exception is present, ordered correctly, and a negative graph is rejected.

**Completion boundary**: This entry records the design-level acceptance contract only. Do not resolve the GitHub thread until the focused result, applicable specialist review, and final repository validation are terminal success for the pinned identity.

## 28. Thread `PRRT_kwDOTN39s86Odf2x` — Add coverage for the remaining B1 spawn controls

- File: `docs/issues/47-security-acceptance-tests.md`
- Line: 24
- Finding basis: This suite covers shell injection and env leakage, but DESIGN §11/B1 also requires cwd validation, per-request/session timeouts, output-size caps, and kill-on-exit. Without explicit tests for those controls, the security gate can still pass while the spawn boundary regresses.

**Normative resolution**: At `docs/issues/47-security-acceptance-tests.md:24`, the canonical contract SHALL adopt the concrete requirement in this finding: This suite covers shell injection and env leakage, but DESIGN §11/B1 also requires cwd validation, per-request/session timeouts, output-size caps, and kill-on-exit. Without explicit tests for those controls, the security gate can still pass while the spawn boundary regresses.. Unsafe, malformed, or unsupported input must fail at the stated boundary without leaking data or creating an unintended side effect.

**Focused verification gate**: Run a safe control and each unsafe/boundary fixture named in the finding for `docs/issues/47-security-acceptance-tests.md:24`; assert rejection/containment, no sensitive-data exposure, and no partial side effect.

**Completion boundary**: This entry records the design-level acceptance contract only. Do not resolve the GitHub thread until the focused result, applicable specialist review, and final repository validation are terminal success for the pinned identity.

## 29. Thread `PRRT_kwDOTN39s86Odf2z` — Call out the replay and persistence controls explicitly

- File: `docs/issues/50-security-md-threat-model.md`
- Line: 18
- Finding basis: The scope says the threat model will cover B1–B5, but it only names generic limitations. DESIGN §11 also requires the replay boundary to stay in-memory/FS-isolated and the persistence boundary to use atomic, restricted writes; those controls need to be spelled out so the threat model matches the security contract.

**Normative resolution**: At `docs/issues/50-security-md-threat-model.md:18`, the canonical contract SHALL adopt the concrete requirement in this finding: The scope says the threat model will cover B1–B5, but it only names generic limitations. DESIGN §11 also requires the replay boundary to stay in-memory/FS-isolated and the persistence boundary to use atomic, restricted writes; those controls need to be spelled out so the threat model matches the security contract.. Unsafe, malformed, or unsupported input must fail at the stated boundary without leaking data or creating an unintended side effect.

**Focused verification gate**: Run the positive control and the exact negative/boundary case named in the finding for `docs/issues/50-security-md-threat-model.md:18`; assert the canonical result and error behavior.

**Completion boundary**: This entry records the design-level acceptance contract only. Do not resolve the GitHub thread until the focused result, applicable specialist review, and final repository validation are terminal success for the pinned identity.

## Specialist handoffs

- `tech-qa`/`tech-tester`: execute all focused gates and repository full-validation; missing, pending, failed, skipped, cancelled, timed-out, or stale evidence blocks closure.
- `tech-security`/`tech-devopssec`: review credentials, egress, spawning, paths, permissions, redaction, ref safety, output limits, and export controls; non-acceptance blocks closure.
- `gate-task-evaluator`: re-pin repository, PR, base/head, merge candidate, required checks, review decision, unresolved threads, policy version, and waiver immediately before merge.

No Bot trigger, review submission, or Bot rerun is authorized.