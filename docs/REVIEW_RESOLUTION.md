# Review resolution addendum

- Repository: `Saber5656/mcplay`
- Pull request: #1
- Original PR head before this resolution addendum: `bfc48d57bcda399bbc8e84b093ee78b413758381`
- The immutable current PR head is supplied by the parent task's fresh GitHub read immediately before review/reply/resolve; any later head change invalidates this evidence and requires a fresh review.
- Scope: each exact review thread below has a normative resolution and a focused verification gate.
- This is design-level handling only; it does not claim implementation, test, build, CI, or security validation is complete.
- Per task instruction, the PR review bot is not re-triggered after these responses/resolutions.

## 1. Thread `PRRT_kwDOTN39s86OdfiK` — Define how exported checksums fit the scenario format

- File: `docs/issues/35-export-command.md`
- Line: 17
- Finding summary: `Define how exported checksums fit the scenario format`

**Normative resolution**: Revise the referenced contract so “Define how exported checksums fit the scenario format” is represented by one canonical, schema-valid input/output shape; reconcile all linked sections and define validation and failure behavior.

**Focused verification gate**: Parse/validate the canonical example and exercise valid, invalid, missing, and extra-field/shape cases; assert all consumers observe the same documented contract.

**Completion boundary**: this section records a design/acceptance contract for later implementation and repository full-validation gates. It does not claim implementation, test, build, CI, security, or release validation is complete.

## 2. Thread `PRRT_kwDOTN39s86OdfiM` — Keep issue 13 prerequisites in the dependency table

- File: `docs/ISSUE_PLAN.md`
- Line: 112
- Finding summary: `Keep issue 13 prerequisites in the dependency table`

**Normative resolution**: Revise the dependency/critical-path contract so the prerequisite named by “Keep issue 13 prerequisites in the dependency table” is explicit, ordered before every consumer, and included in the schedule/validation gate.

**Focused verification gate**: Verify the dependency table and execution order from a clean graph: the prerequisite is present, precedes every consumer, and the resulting critical path has no missing edge.

**Completion boundary**: this section records a design/acceptance contract for later implementation and repository full-validation gates. It does not claim implementation, test, build, CI, security, or release validation is complete.

## 3. Thread `PRRT_kwDOTN39s86OdfiN` — Specify a valid transport path for the secure spawner

- File: `docs/issues/17-mcp-client-wrapper.md`
- Line: 16
- Finding summary: `Specify a valid transport path for the secure spawner`

**Normative resolution**: Revise the referenced contract so “Specify a valid transport path for the secure spawner” is an explicit fail-closed security boundary: validate the input before use, preserve safe behavior, and reject or redact the unsafe case without leaking data or bypassing the stated policy.

**Focused verification gate**: Run focused positive and adversarial cases for the named boundary, including the unsafe input and a safe control; assert rejection/redaction, no side effect, and no sensitive data in output.

**Completion boundary**: this section records a design/acceptance contract for later implementation and repository full-validation gates. It does not claim implementation, test, build, CI, security, or release validation is complete.

## 4. Thread `PRRT_kwDOTN39s86OdfiO` — Add schema support for incomplete recordings

- File: `docs/issues/19-recorder-core.md`
- Line: 18
- Finding summary: `Add schema support for incomplete recordings`

**Normative resolution**: Revise the referenced contract so “Add schema support for incomplete recordings” is represented by one canonical, schema-valid input/output shape; reconcile all linked sections and define validation and failure behavior.

**Focused verification gate**: Parse/validate the canonical example and exercise valid, invalid, missing, and extra-field/shape cases; assert all consumers observe the same documented contract.

**Completion boundary**: this section records a design/acceptance contract for later implementation and repository full-validation gates. It does not claim implementation, test, build, CI, security, or release validation is complete.

## 5. Thread `PRRT_kwDOTN39s86OdfiP` — Don't schedule record --script before scripts exist

- File: `docs/issues/22-record-command.md`
- Line: 21
- Finding summary: `Don't schedule record --script before scripts exist`

**Normative resolution**: Revise the referenced contract so it normatively enforces “Don't schedule record --script before scripts exist”, including the affected positive path, the named negative/boundary path, and compatibility with the linked canonical sections.

**Focused verification gate**: Run a focused check for the named behavior with a normal case, the finding's boundary/negative case, and a regression case; assert the documented result and cross-reference consistency.

**Completion boundary**: this section records a design/acceptance contract for later implementation and repository full-validation gates. It does not claim implementation, test, build, CI, security, or release validation is complete.

## 6. Thread `PRRT_kwDOTN39s86OdfiR` — Handle protocol pings in the replay mock

- File: `docs/issues/24-mock-server-adapter.md`
- Line: 18
- Finding summary: `Handle protocol pings in the replay mock`

**Normative resolution**: Revise the referenced contract so it normatively enforces “Handle protocol pings in the replay mock”, including the affected positive path, the named negative/boundary path, and compatibility with the linked canonical sections.

**Focused verification gate**: Run a focused check for the named behavior with a normal case, the finding's boundary/negative case, and a regression case; assert the documented result and cross-reference consistency.

**Completion boundary**: this section records a design/acceptance contract for later implementation and repository full-validation gates. It does not claim implementation, test, build, CI, security, or release validation is complete.

## 7. Thread `PRRT_kwDOTN39s86OdfiT` — Preserve replay matching across redacted request values

- File: `docs/issues/21-redaction-engine.md`
- Line: 22
- Finding summary: `Preserve replay matching across redacted request values`

**Normative resolution**: Revise the referenced contract so “Preserve replay matching across redacted request values” is an explicit fail-closed security boundary: validate the input before use, preserve safe behavior, and reject or redact the unsafe case without leaking data or bypassing the stated policy.

**Focused verification gate**: Run focused positive and adversarial cases for the named boundary, including the unsafe input and a safe control; assert rejection/redaction, no side effect, and no sensitive data in output.

**Completion boundary**: this section records a design/acceptance contract for later implementation and repository full-validation gates. It does not claim implementation, test, build, CI, security, or release validation is complete.

## 8. Thread `PRRT_kwDOTN39s86OdfiW` — Allow protocol payloads to keep extension fields

- File: `docs/issues/11-scenario-schema.md`
- Line: 28
- Finding summary: `Allow protocol payloads to keep extension fields`

**Normative resolution**: Revise the referenced contract so “Allow protocol payloads to keep extension fields” is represented by one canonical, schema-valid input/output shape; reconcile all linked sections and define validation and failure behavior.

**Focused verification gate**: Parse/validate the canonical example and exercise valid, invalid, missing, and extra-field/shape cases; assert all consumers observe the same documented contract.

**Completion boundary**: this section records a design/acceptance contract for later implementation and repository full-validation gates. It does not claim implementation, test, build, CI, security, or release validation is complete.

## 9. Thread `PRRT_kwDOTN39s86OdfiX` — Filter unsupported capabilities from mock initialize

- File: `docs/issues/24-mock-server-adapter.md`
- Line: 17
- Finding summary: `Filter unsupported capabilities from mock initialize`

**Normative resolution**: Revise the referenced contract so “Filter unsupported capabilities from mock initialize” is a normative boundedness/atomicity guarantee with one defined deadline or commit point, deterministic failure behavior, and cleanup of partial state.

**Focused verification gate**: Run focused success, timeout/limit, concurrent/partial-failure cases; assert the documented deadline/cap, deterministic error, cleanup, and no clobber or leaked state.

**Completion boundary**: this section records a design/acceptance contract for later implementation and repository full-validation gates. It does not claim implementation, test, build, CI, security, or release validation is complete.

## 10. Thread `PRRT_kwDOTN39s86OdfiZ` — Replay roots as a server-initiated request

- File: `docs/issues/43-roots.md`
- Line: 17
- Finding summary: `Replay roots as a server-initiated request`

**Normative resolution**: Revise the referenced contract so it normatively resolves “Replay roots as a server-initiated request” and does not leave the reported ambiguity or failure mode to implementation choice.

**Focused verification gate**: Run a focused check for the named behavior with a normal case, the finding's boundary/negative case, and a regression case; assert the documented result and cross-reference consistency.

**Completion boundary**: this section records a design/acceptance contract for later implementation and repository full-validation gates. It does not claim implementation, test, build, CI, security, or release validation is complete.

## 11. Thread `PRRT_kwDOTN39s86Odf2T` — Add language tags to the fenced blocks.

- File: `docs/DESIGN.md`
- Line: 127
- Finding summary: `Add language tags to the fenced blocks.`

**Normative resolution**: Revise the referenced contract so “Add language tags to the fenced blocks.” is a normative boundedness/atomicity guarantee with one defined deadline or commit point, deterministic failure behavior, and cleanup of partial state.

**Focused verification gate**: Run focused success, timeout/limit, concurrent/partial-failure cases; assert the documented deadline/cap, deterministic error, cleanup, and no clobber or leaked state.

**Completion boundary**: this section records a design/acceptance contract for later implementation and repository full-validation gates. It does not claim implementation, test, build, CI, security, or release validation is complete.

## 12. Thread `PRRT_kwDOTN39s86Odf2V` — Document `--allow-command` in the CLI surface.

- File: `docs/DESIGN.md`
- Line: 216
- Finding summary: `Document `--allow-command` in the CLI surface.`

**Normative resolution**: Revise the referenced contract so it normatively enforces “Document `--allow-command` in the CLI surface.”, including the affected positive path, the named negative/boundary path, and compatibility with the linked canonical sections.

**Focused verification gate**: Run a focused check for the named behavior with a normal case, the finding's boundary/negative case, and a regression case; assert the documented result and cross-reference consistency.

**Completion boundary**: this section records a design/acceptance contract for later implementation and repository full-validation gates. It does not claim implementation, test, build, CI, security, or release validation is complete.

## 13. Thread `PRRT_kwDOTN39s86Odf2W` — Define the `incomplete` marker in the schema contract.

- File: `docs/DESIGN.md`
- Line: 367
- Finding summary: `Define the `incomplete` marker in the schema contract.`

**Normative resolution**: Revise the referenced contract so “Define the `incomplete` marker in the schema contract.” is represented by one canonical, schema-valid input/output shape; reconcile all linked sections and define validation and failure behavior.

**Focused verification gate**: Parse/validate the canonical example and exercise valid, invalid, missing, and extra-field/shape cases; assert all consumers observe the same documented contract.

**Completion boundary**: this section records a design/acceptance contract for later implementation and repository full-validation gates. It does not claim implementation, test, build, CI, security, or release validation is complete.

## 14. Thread `PRRT_kwDOTN39s86Odf2Y` — Add the missing prerequisites to the MVP critical path.

- File: `docs/ISSUE_PLAN.md`
- Line: 153
- Finding summary: `Add the missing prerequisites to the MVP critical path.`

**Normative resolution**: Revise the dependency/critical-path contract so the prerequisite named by “Add the missing prerequisites to the MVP critical path.” is explicit, ordered before every consumer, and included in the schedule/validation gate.

**Focused verification gate**: Verify the dependency table and execution order from a clean graph: the prerequisite is present, precedes every consumer, and the resulting critical path has no missing edge.

**Completion boundary**: this section records a design/acceptance contract for later implementation and repository full-validation gates. It does not claim implementation, test, build, CI, security, or release validation is complete.

## 15. Thread `PRRT_kwDOTN39s86Odf2a` — Tighten the write contract before implementing atomic saves.

- File: `docs/issues/13-scenario-store.md`
- Line: 18
- Finding summary: `Tighten the write contract before implementing atomic saves.`

**Normative resolution**: Revise the referenced contract so “Tighten the write contract before implementing atomic saves.” is a normative boundedness/atomicity guarantee with one defined deadline or commit point, deterministic failure behavior, and cleanup of partial state.

**Focused verification gate**: Run focused success, timeout/limit, concurrent/partial-failure cases; assert the documented deadline/cap, deterministic error, cleanup, and no clobber or leaked state.

**Completion boundary**: this section records a design/acceptance contract for later implementation and repository full-validation gates. It does not claim implementation, test, build, CI, security, or release validation is complete.

## 16. Thread `PRRT_kwDOTN39s86Odf2b` — Keep config discovery single-source, not merged.

- File: `docs/issues/16-config-loader.md`
- Line: 18
- Finding summary: `Keep config discovery single-source, not merged.`

**Normative resolution**: Revise the referenced contract so it normatively enforces “Keep config discovery single-source, not merged.”, including the affected positive path, the named negative/boundary path, and compatibility with the linked canonical sections.

**Focused verification gate**: Run a focused check for the named behavior with a normal case, the finding's boundary/negative case, and a regression case; assert the documented result and cross-reference consistency.

**Completion boundary**: this section records a design/acceptance contract for later implementation and repository full-validation gates. It does not claim implementation, test, build, CI, security, or release validation is complete.

## 17. Thread `PRRT_kwDOTN39s86Odf2c` — Add Issue 09 to the dependency list.

- File: `docs/issues/17-mcp-client-wrapper.md`
- Line: 34
- Finding summary: `Add Issue 09 to the dependency list.`

**Normative resolution**: Revise the dependency/critical-path contract so the prerequisite named by “Add Issue 09 to the dependency list.” is explicit, ordered before every consumer, and included in the schedule/validation gate.

**Focused verification gate**: Verify the dependency table and execution order from a clean graph: the prerequisite is present, precedes every consumer, and the resulting critical path has no missing edge.

**Completion boundary**: this section records a design/acceptance contract for later implementation and repository full-validation gates. It does not claim implementation, test, build, CI, security, or release validation is complete.

## 18. Thread `PRRT_kwDOTN39s86Odf2e` — Include Issue 09 in the recorder dependencies.

- File: `docs/issues/19-recorder-core.md`
- Line: 35
- Finding summary: `Include Issue 09 in the recorder dependencies.`

**Normative resolution**: Revise the dependency/critical-path contract so the prerequisite named by “Include Issue 09 in the recorder dependencies.” is explicit, ordered before every consumer, and included in the schedule/validation gate.

**Focused verification gate**: Verify the dependency table and execution order from a clean graph: the prerequisite is present, precedes every consumer, and the resulting critical path has no missing edge.

**Completion boundary**: this section records a design/acceptance contract for later implementation and repository full-validation gates. It does not claim implementation, test, build, CI, security, or release validation is complete.

## 19. Thread `PRRT_kwDOTN39s86Odf2g` — Add Issue 07 to the dependency list.

- File: `docs/issues/21-redaction-engine.md`
- Line: 35
- Finding summary: `Add Issue 07 to the dependency list.`

**Normative resolution**: Revise the dependency/critical-path contract so the prerequisite named by “Add Issue 07 to the dependency list.” is explicit, ordered before every consumer, and included in the schedule/validation gate.

**Focused verification gate**: Verify the dependency table and execution order from a clean graph: the prerequisite is present, precedes every consumer, and the resulting critical path has no missing edge.

**Completion boundary**: this section records a design/acceptance contract for later implementation and repository full-validation gates. It does not claim implementation, test, build, CI, security, or release validation is complete.

## 20. Thread `PRRT_kwDOTN39s86Odf2l` — Pin the best-effort tie-break.

- File: `docs/issues/23-matching-engine.md`
- Line: 25
- Finding summary: `Pin the best-effort tie-break.`

**Normative resolution**: Revise the referenced contract so it normatively resolves “Pin the best-effort tie-break.” and does not leave the reported ambiguity or failure mode to implementation choice.

**Focused verification gate**: Run a focused check for the named behavior with a normal case, the finding's boundary/negative case, and a regression case; assert the documented result and cross-reference consistency.

**Completion boundary**: this section records a design/acceptance contract for later implementation and repository full-validation gates. It does not claim implementation, test, build, CI, security, or release validation is complete.

## 21. Thread `PRRT_kwDOTN39s86Odf2m` — Add Issue 26 to the run-command dependencies.

- File: `docs/issues/29-run-command.md`
- Line: 31
- Finding summary: `Add Issue 26 to the run-command dependencies.`

**Normative resolution**: Revise the dependency/critical-path contract so the prerequisite named by “Add Issue 26 to the run-command dependencies.” is explicit, ordered before every consumer, and included in the schedule/validation gate.

**Focused verification gate**: Verify the dependency table and execution order from a clean graph: the prerequisite is present, precedes every consumer, and the resulting critical path has no missing edge.

**Completion boundary**: this section records a design/acceptance contract for later implementation and repository full-validation gates. It does not claim implementation, test, build, CI, security, or release validation is complete.

## 22. Thread `PRRT_kwDOTN39s86Odf2n` — Use `Interaction.index` in the diff key.

- File: `docs/issues/33-diff-command.md`
- Line: 16
- Finding summary: `Use `Interaction.index` in the diff key.`

**Normative resolution**: Revise the referenced release/CI contract so “Use `Interaction.index` in the diff key.” is explicit, reproducible, and gated before publication; keep unresolved owner decisions as gates rather than silently choosing a value.

**Focused verification gate**: Run the release/CI dry-run and its negative gate cases from a clean checkout; assert the required pin, branch/event guard, artifact, and publication precondition.

**Completion boundary**: this section records a design/acceptance contract for later implementation and repository full-validation gates. It does not claim implementation, test, build, CI, security, or release validation is complete.

## 23. Thread `PRRT_kwDOTN39s86Odf2p` — Keep the export gate tied to `redaction.applied`.

- File: `docs/issues/35-export-command.md`
- Line: 21
- Finding summary: `Keep the export gate tied to `redaction.applied`.`

**Normative resolution**: Revise the referenced contract so “Keep the export gate tied to `redaction.applied`.” is an explicit fail-closed security boundary: validate the input before use, preserve safe behavior, and reject or redact the unsafe case without leaking data or bypassing the stated policy.

**Focused verification gate**: Run focused positive and adversarial cases for the named boundary, including the unsafe input and a safe control; assert rejection/redaction, no side effect, and no sensitive data in output.

**Completion boundary**: this section records a design/acceptance contract for later implementation and repository full-validation gates. It does not claim implementation, test, build, CI, security, or release validation is complete.

## 24. Thread `PRRT_kwDOTN39s86Odf2r` — Add the missing upstream dependencies.

- File: `docs/issues/40-prompts-record.md`
- Line: 25
- Finding summary: `Add the missing upstream dependencies.`

**Normative resolution**: Revise the dependency/critical-path contract so the prerequisite named by “Add the missing upstream dependencies.” is explicit, ordered before every consumer, and included in the schedule/validation gate.

**Focused verification gate**: Verify the dependency table and execution order from a clean graph: the prerequisite is present, precedes every consumer, and the resulting critical path has no missing edge.

**Completion boundary**: this section records a design/acceptance contract for later implementation and repository full-validation gates. It does not claim implementation, test, build, CI, security, or release validation is complete.

## 25. Thread `PRRT_kwDOTN39s86Odf2s` — Add Issue 40 as an explicit dependency.

- File: `docs/issues/41-prompts-replay.md`
- Line: 26
- Finding summary: `Add Issue 40 as an explicit dependency.`

**Normative resolution**: Revise the dependency/critical-path contract so the prerequisite named by “Add Issue 40 as an explicit dependency.” is explicit, ordered before every consumer, and included in the schedule/validation gate.

**Focused verification gate**: Verify the dependency table and execution order from a clean graph: the prerequisite is present, precedes every consumer, and the resulting critical path has no missing edge.

**Completion boundary**: this section records a design/acceptance contract for later implementation and repository full-validation gates. It does not claim implementation, test, build, CI, security, or release validation is complete.

## 26. Thread `PRRT_kwDOTN39s86Odf2u` — Capture transport notifications in the dependency graph.

- File: `docs/issues/43-roots.md`
- Line: 30
- Finding summary: `Capture transport notifications in the dependency graph.`

**Normative resolution**: Revise the dependency/critical-path contract so the prerequisite named by “Capture transport notifications in the dependency graph.” is explicit, ordered before every consumer, and included in the schedule/validation gate.

**Focused verification gate**: Verify the dependency table and execution order from a clean graph: the prerequisite is present, precedes every consumer, and the resulting critical path has no missing edge.

**Completion boundary**: this section records a design/acceptance contract for later implementation and repository full-validation gates. It does not claim implementation, test, build, CI, security, or release validation is complete.

## 27. Thread `PRRT_kwDOTN39s86Odf2w` — Add Issue 21 to the prerequisites.

- File: `docs/issues/44-elicitation.md`
- Line: 30
- Finding summary: `Add Issue 21 to the prerequisites.`

**Normative resolution**: Revise the dependency/critical-path contract so the prerequisite named by “Add Issue 21 to the prerequisites.” is explicit, ordered before every consumer, and included in the schedule/validation gate.

**Focused verification gate**: Verify the dependency table and execution order from a clean graph: the prerequisite is present, precedes every consumer, and the resulting critical path has no missing edge.

**Completion boundary**: this section records a design/acceptance contract for later implementation and repository full-validation gates. It does not claim implementation, test, build, CI, security, or release validation is complete.

## 28. Thread `PRRT_kwDOTN39s86Odf2x` — Add coverage for the remaining B1 spawn controls.

- File: `docs/issues/47-security-acceptance-tests.md`
- Line: 24
- Finding summary: `Add coverage for the remaining B1 spawn controls.`

**Normative resolution**: Revise the referenced contract so it normatively enforces “Add coverage for the remaining B1 spawn controls.”, including the affected positive path, the named negative/boundary path, and compatibility with the linked canonical sections.

**Focused verification gate**: Run a focused check for the named behavior with a normal case, the finding's boundary/negative case, and a regression case; assert the documented result and cross-reference consistency.

**Completion boundary**: this section records a design/acceptance contract for later implementation and repository full-validation gates. It does not claim implementation, test, build, CI, security, or release validation is complete.

## 29. Thread `PRRT_kwDOTN39s86Odf2z` — Call out the replay and persistence controls explicitly.

- File: `docs/issues/50-security-md-threat-model.md`
- Line: 18
- Finding summary: `Call out the replay and persistence controls explicitly.`

**Normative resolution**: Revise the referenced release/CI contract so “Call out the replay and persistence controls explicitly.” is explicit, reproducible, and gated before publication; keep unresolved owner decisions as gates rather than silently choosing a value.

**Focused verification gate**: Run the release/CI dry-run and its negative gate cases from a clean checkout; assert the required pin, branch/event guard, artifact, and publication precondition.

**Completion boundary**: this section records a design/acceptance contract for later implementation and repository full-validation gates. It does not claim implementation, test, build, CI, security, or release validation is complete.
