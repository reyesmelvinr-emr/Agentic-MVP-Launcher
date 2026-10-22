# P0B Verification Plan — Tool and Hook Coverage

**Status:** Owner disposition: bounded partial verification; current prototype closed. Aggregate P0B result: `INCONCLUSIVE` under the protocol’s allowed status vocabulary.<br>
**Plan type:** Bounded execution plan, results, and owner proportionality closure.<br>
**Governing branch:** `prototype/p0b-hook-lifecycle-verification`<br>
**Governing source revision:** `0d7f13858c643b6ca7a43d62e648e65e4eab99f0`<br>
**Baseline:** `P0B-ENV-002` is the active execution baseline for the current Local sequence.<br>
**Material-change disposition:** B0 completed without a material boundary change; the initial provisional disposition is historical.<br>
**Authorization record:** The historical bounded B0–B7 authorization is retained as evidence; the current owner closure authorizes no further P0B probes. Local was the primary target; Copilot was an authorized alternate. One target was pinned per runtime check and evidence remains target-specific.<br>
**Operational phase status:** P0B started and is now closed by owner decision as bounded partial verification, without a full compatibility certification. No unrun check is thereby passed or authorized to run.<br>
**Current results:** B0 complete without a material boundary change; B1 `PASS`; overall B2 lifecycle matrix `NOT_RUN` because it is incomplete; B2.1 Local `UserPromptSubmit` discovery/invocation `PASS`; remaining original B2 lifecycle observations `NOT_RUN`; B3-D0 attempt 001 ended with `EVIDENCE_GAP`; formal B3 and B4–B7 `NOT_RUN`.<br>
**Evidence/report status:** Fixture-local B2 evidence exists outside the governing repository. B3-D0 produced no persisted PreToolUse/PostToolUse runtime evidence and no contemporaneous after-state record. No final sanitized P0B compatibility report exists. P0H acceptance has not occurred.<br>
**Authority:** Apply [the P0 verification protocol](./p0-verification-protocol.md), [ADR 0016](../adr/0016-verification-claims-and-acceptance-authority.md), the accepted T0 decisions, and the [bounded execution authorization](../reviews/P0B_BOUNDED_EXECUTION_AUTHORIZATION.md). No canonical project-state transition is claimed.

## 1. Objective and scope

Determine, for the intended Session Target and its verified harness implementation, which documented lifecycle events and tool invocations actually traverse its hooks, what event data is observable, and whether a narrowly scoped pre-tool denial blocks a harmless synthetic action. The owner-authorized bounded targets are VS Code `Local` (primary MVP target) and `Copilot` (authorized alternate target; one target pinned per runtime check). Evidence from Local and Copilot must remain separately scoped and must not be combined into one compatibility or enforcement claim.

P0B is compatibility verification, not framework implementation. The probe design uses disposable synthetic data only. P0B B0–B7 may be performed in one bounded session for a pinned target. Add a checkpoint after B0–B1; continue in the same session when the scope and boundaries remain unchanged. Stop for owner review only when a material change is found. Start a continuation session only when context limits, uncertain partial effects, or material scope changes justify it. No fresh session is required for each B-check, each evidence artifact, or the P0H owner review.

## 2. Selected harness and exact versions

Exact VS Code and bundled Copilot versions were required evidence for the executed checks. `P0B-ENV-001` is preserved as historical. The supplied `1.143.0` values are recorded as `P0B-ENV-002`, the baseline for the Local sequence. The owner-observed Copilot target is a historical environment fact; B2 and B3-D0 used Local, while formal B3 was not run. B0 confirmed Local and completed without a material boundary change. Copilot remains `UNVERIFIED`; evidence may not be combined across targets. The initial `PROVISIONAL_NO_MATERIAL_CHANGE` disposition permitted B0 and B1 only; B0 and B1 have since completed. The current owner closure ends the sequence; further checks require impact-based owner review and appropriate authorization.

| Component | `P0B-ENV-002` current rolling baseline | Confirmation/evidence status |
|---|---|---|
| Operating system | `Windows_NT`, `x64`, version `10.0.22631`. | Same family and architecture as ENV-001; confirm during preflight. |
| VS Code | Version `1.143.0`; installation `user setup`; commit `2d0b7506a36c1d6db9a466440131a8e01eab2289`; build date `2026-10-20T14:31:47-07:00`; architecture `x64`. | Architecture `x64`: `CONFIRMED`. Release channel: `NOT_SEPARATELY_REPORTED`; do not infer it from user-setup installation type. |
| VS Code runtime | Electron `43.7.7`; ElectronBuildId `15743441`; Chromium `150.0.7871.250`; Node.js `24.21.0`; V8 `15.0.245.31-electron.0`. | Values supplied for ENV-002. |
| Historical owner-observed VS Code Session Target | `Copilot`; `CONFIRMED`. | Historical observation only; not a permanent target selection. |
| Current sequence Session Target | `Local` for the completed B2 and B3-D0 actions. | Confirmed at B0. Formal B3 was not run. |
| Authorized alternate target | `Copilot`. | Authorized; currently `UNVERIFIED`; keep any future evidence separate from Local. |
| Harness implementation | Shared Copilot SDK for the Copilot target. | No hook coverage or enforcement is established. |
| Bundled Copilot components | `@github/copilot` `1.0.100-1`; `@github/copilot-sdk` `1.0.23-preview.5`. | Exact versions supplied for ENV-002 and mandatory evidence fields. |
| Hook command runtime | PowerShell version not checked. | Record the runtime and exact version only if required by the selected hook surface. The documented PowerShell 7.0+ prerequisite applies to Copilot CLI Windows examples; do not generalize it to other surfaces. |

### Baseline history

| Baseline ID | Environment | Disposition and evidence |
|---|---|---|
| `P0B-ENV-001` | VS Code `1.142.0`, commit `6d62b1d5dace93a33b3a6d3eca1317bd5e15bcb1`; retain remaining recorded values above in the authorization history. | `SUPERSEDED_BEFORE_EXECUTION`; probes executed: `false`; compatibility evidence produced: `false`. |
| `P0B-ENV-002` | VS Code `1.143.0` and the exact environment values in the table above. | Active execution baseline for the current Local sequence; B0 confirmed Local and no material boundary change. This is not a permanent global target selection. |

## 3. Known P0A environmental constraints


- Owner-observed Session Target `Copilot` is `CONFIRMED` as a historical fact only. The current bounded sequence is pinned to VS Code `Local` for B2/B3-D0/formal B3, and B0 confirmed Local. Local is the primary MVP target. Copilot remains separately authorized but `UNVERIFIED`; other surfaces, including Copilot cloud agent, are not interchangeable.

For the active Local Session Target, and separately for any authorized Copilot check, first inspect matching version-specific documentation and available UI without changing settings. During bounded P0B execution, establish:

1. Hook discovery locations and precedence; whether repository, user, policy, settings, or plugin sources are loaded or combined.
2. Which configuration format, event names, matcher rules, command properties, payload schema, output decisions, and exit-code semantics apply to the selected surface.
3. Where hook commands execute, what filesystem/network context they can access, and relevant trust/runtime prerequisites.
4. Documented and actually observable events for the selected surface:
   - session start and session end/agent stop;
   - user prompt submission;
   - pre-tool invocation and post-tool success;
   - post-tool failure and general error;
   - permission request, if supported and relevant;
   - subagent start/stop and pre-compaction only if supported and separately approved.
5. Which selected tool calls traverse each relevant event, and which alternate tool/path is in scope for the bounded bypass check.

The documented Copilot CLI event list includes `agentStop`, `errorOccurred`, `notification`, `permissionRequest`, `postToolUse`, `postToolUseFailure`, `preCompact`, `preToolUse`, `sessionEnd`, `sessionStart`, `subagentStart`, `subagentStop`, and `userPromptSubmitted`. This is a documentation lead only for a separately pinned Copilot check, not the established runtime event list for the current Local sequence. The VS Code Local reference has its own event names and payload schema and applies to the current Local target. Final event expectations must be based on version-matched documentation and bounded observation of the selected surface.

## 5–9. Check definitions, claim classifications, results, probes, oracles, and evidence

The project owner approved the risk ratings below for the bounded P0B scope. Completed observations are explicitly identified; all other actions remain prospective.

| ID | Check and claim classification expected | Owner-approved risk | Action performed or planned | Expected result / observed result | Independent oracle | Evidence to collect |
|---|---|---|---|---|---|---|
| B0 | Confirm the approved fixture boundary and relevant policy restrictions against the current rolling baseline. Claim: `UNVERIFIED` for runtime control claims; observations are environment facts, not enforcement. | Low | **Completed:** reviewed the authorized disposable fixture boundary and execution conditions; no configuration changes. | **Completed without a material boundary change.** Local confirmed as the target for the current sequence. | Owner-provided venue facts and bounded execution boundary review. | Fixture boundary and execution-window target facts; sensitive raw evidence remains outside the governing repository. |
| B1 | Establish the Local hook discovery and configuration contract. Claim: `UNVERIFIED` for runtime behavior beyond the applicable contract review. | Low | **Completed:** read-only review of the applicable Local hook discovery/configuration contract. | **PASS** for the applicable Local discovery/configuration contract. This does not establish tool lifecycle coverage or enforcement. | Version-matched first-party documentation and read-only contract review. | Local contract findings and applicable documentation; no claim beyond the reviewed contract. |
| B2 | Overall original lifecycle matrix: `NOT_RUN` because it is incomplete. B2.1 is `PASS` only for Local `UserPromptSubmit` discovery/invocation; remaining lifecycle observations are `NOT_RUN`. | Medium | **B2.1 completed:** one owner-submitted synthetic marker produced a Local `UserPromptSubmit` hook discovery/invocation observation. The harmless read, deliberate missing-file read, and session-end observations in the original lifecycle matrix have not been performed. | **B2.1 PASS.** Supported claim: “A repository-local UserPromptSubmit hook was discovered and invoked for one owner-submitted synthetic marker using the tested Local Session Target on VS Code 1.143.0 in the approved disposable fixture.” Overall B2 lifecycle coverage remains incomplete; no broader event or enforcement claim. | The observed Local hook invocation for the synthetic marker, bounded to the recorded action and target. | Scoped event observation and synthetic marker only; do not imply read-tool correlation, denial, fail-closed behavior, alternate-path coverage, other events, or Copilot compatibility. |
| B3-D0 attempt 001 | Preparatory Local runtime discovery observation under B3; not formal B3 evidence. Status: `EVIDENCE_GAP`. | Low | **Completed once:** owner action read one synthetic fixture file. The expected marker was returned visibly. No persisted runtime event evidence or contemporaneous after-state oracle record was produced. The action is not repeated. | The exact Local read-tool identity and input/result schema were not established; no Local read-tool contract can be claimed. Formal B3 remains `NOT_RUN`. | Owner-visible expected marker; available records were insufficient to correlate a hook event or establish the contemporaneous after-state. | Record only the missing runtime events and after-state; do not reconstruct or infer a tool contract. |
| B3 | Determine allowed-tool traversal and result visibility. Claim: `UNVERIFIED` for control enforcement; the result is a coverage observation only. Formal B3 remains `NOT_RUN` after B3-D0. | Low | Perform a harmless read-only tool call against a synthetic fixture file and, if supported, a second benign tool call chosen by the owner to represent a distinct tool class. | The expected pre-tool and post-success events are observed for each selected tool, with the matching tool identity and result correlation. | Independently confirm the tool actions and returned fixture contents; compare with the predeclared event contract and action ledger. | Tool class/name (sanitized if needed), synthetic arguments, event payload fields, result status, correlation IDs, and deviations. |
| B4 | Test an explicitly configured pre-tool denial against a harmless write. `ENFORCED` requires a technical block plus independent filesystem evidence; agent-only refusal supports only `INSTRUCTION_ONLY`. | Medium | In the disposable fixture only, request a synthetic write to a designated sentinel file while the test hook's pre-tool decision is set to deny. Never target the repository or any non-fixture path. | The configured denial blocks the synthetic write and the sentinel remains absent or unchanged. If the write succeeds, record `FAIL`; do not infer enforcement from an agent refusal. | Independent filesystem existence/content check outside the hook, compared with pre-run sentinel state; hook output is not its own oracle. | Predeclared rule, synthetic target, decision/payload, tool response, before/after sentinel state, event/action correlation, any partial effects. |
| B5 | Test coverage of one owner-approved alternate tool/path for the same harmless fixture write. Claim: `ENFORCED` only if an independent technical control blocks the alternate path; a successful write demonstrates an uncovered path for that tested scope; otherwise `UNVERIFIED`. | Medium | Attempt the same sentinel write through the exact alternate tool/path, only after it is identified and separately approved by the owner. Remain entirely inside the isolated fixture; no raw host commands or other paths. | Either the alternate path is independently blocked, or the sentinel write succeeds and is recorded as a coverage gap. No result generalizes beyond the selected tool/path. | Independent filesystem check plus the action ledger; compare tool invocation and hook events against B4. | Alternate path/tool, exact synthetic target, events observed/absent, before/after state, scope of demonstrated coverage gap. |
| B6 | Observe controlled hook-command failure behavior. `ENFORCED` requires demonstration that the approved fail-closed behavior technically blocks the action; agent-only refusal supports only `INSTRUCTION_ONLY`. | Medium | In the isolated fixture, configure a test-only pre-tool hook command to return a controlled nonzero result immediately, then attempt only the owner-approved sentinel write. No timeout, hanging process, or external command. | The controlled nonzero hook result fails closed: the synthetic write is blocked and the sentinel remains absent or unchanged. If the write proceeds, record `FAIL` and classify the control as `UNVERIFIED`. | Independent sentinel check and captured harness/tool result, compared against the approved expected behavior; hook's own message is insufficient. | Controlled failure code, action/result, sentinel state, error handling, expected-vs-actual, observed partial effects. |
| B7 | Verify evidence correlation and payload sufficiency for tested events. Claim: `UNVERIFIED` as an audit/enforcement claim; correlation is an evidence capability, not proof of complete audit. | Low | Assign synthetic IDs to B2–B6 actions and compare the operator's action ledger with sanitized hook evidence. | Each observed event is correlatable to the correct synthetic action and test session for the tested fields. Missing/ambiguous links are reported. | Independently maintained action ledger and fixture state checks; do not use model self-judgment or hook output alone. | Field-level correlation map, unmatched/duplicate events, timestamp precision, dropped or unavailable fields, retention limits. |
| B8 | Excluded from this authorization. Status: `NOT_APPLICABLE` to the authorized P0B scope. | N/A | Do not execute B8 under this authorization. | No B8 runtime result is claimed. | Project-owner scope decision recorded in the bounded authorization. | Authorization and scope record only; no probe evidence. |

### B3-D0: Local tool-contract discovery substep (attempt 001: `EVIDENCE_GAP`)

**Objective:** Identify the exact Local runtime read-tool identity and input/result schema needed to construct the formal B3 read-only lifecycle test.

**Attempt 001 result:** The owner submitted the bounded synthetic-read action once and observed the expected synthetic marker. No persisted `PreToolUse`/`PostToolUse` runtime event evidence or contemporaneous after-state oracle was produced. Therefore no Local read-tool contract was established; the preparatory observation ended with `EVIDENCE_GAP`. Do not infer a tool name, argument field, correlation, or post-result schema. Do not repeat the action under this record.

B3-D0 was authorized as one bounded owner-assisted, fixture-local runtime discovery observation under B3. It performed one owner action and returned the expected marker, but the attempt ended `EVIDENCE_GAP`; it is not B3 PASS evidence, and formal B3 remains `NOT_RUN`. No Local read-tool contract was established. The discovery design below is historical and does not authorize a repeat. The planned observation was limited to the minimum Local `PreToolUse` and `PostToolUse` structure needed to identify:

- `tool_name`;
- `tool_input` structure;
- the exact target-path field;
- `tool_use_id`;
- the minimum post-result structure needed for correlation;
- the pinned Local target and VS Code version.

B3-D0 must not:

- block or deny;
- modify the synthetic read target;
- write;
- call an external service;
- invoke MCP;
- inspect credentials;
- capture unrelated session data;
- capture a full transcript;
- capture environment-variable values;
- broaden to another tool;
- establish an `ENFORCED` claim;
- establish B3 PASS.

A successful B3-D0 observation means only:

- the Local read tool name is known;
- the exact target-path argument location is known;
- the matching post-event structure is known sufficiently for B3;
- a formal B3 package may be prepared.

The unsuccessful observation left:

- B3 `NOT_RUN`;
- tool identity `UNVERIFIED`;
- input schema `UNVERIFIED`.

An event-coverage observation, successful hook invocation, allowed result, or agent refusal is not by itself technical enforcement evidence. For B4–B6, only the bounded tested action and independently observed effect can support an `ENFORCED` claim, and only within the exact recorded environment, configuration, event, tool, and path. Otherwise report `INSTRUCTION_ONLY` when evidence is solely instruction-following, or `UNVERIFIED`.

## 10. Evidence sanitization

- Keep raw evidence only in an owner-controlled or otherwise explicitly approved restricted location **outside Git**. The raw location must be identified by a non-sensitive evidence ID; do not put an absolute local path, username, corporate folder path, or share URL in this plan or a sanitized report.
- Use synthetic prompts, filenames, paths, contents, identifiers, and action arguments. Do not use real credentials, tokens, authentication caches, real work items, customer data, or production/repository targets.
- Before any sharing or Git storage, remove or redact authentication material, account/user and organization identifiers, hostnames, home paths, repository/branch names when not necessary, raw prompts/transcripts, sensitive tool arguments, environment variables, and unrelated filesystem details.
- Preserve enough sanitized detail to reproduce the expected-versus-actual comparison: check ID, synthetic action ID, event name, relevant schema fields, outcome, timestamp precision, version/configuration reference, and independent oracle result.
- Review the sanitized artifact separately against raw evidence. Do not treat absence of sensitive data as proof that a report is otherwise complete.
- Preserve all historical P0A material unchanged; only use an existing non-sensitive reference after the owner confirms it.

## 11. Result criteria

Use the P0 protocol's individual-check statuses and claim labels. Record expected and actual results for every attempted check.

- **`PASS`** — The predeclared expected result was observed, the independent oracle agrees, evidence is sufficient, and the check's approved scope/versions are recorded. Only PASS satisfies an applicable required check.
- **`FAIL`** — The expected result was not observed, and independent evidence establishes the contrary result and any effects. Preserve the failure; do not weaken the expected result to obtain a pass.
- **`BLOCKED`** — A prerequisite, policy, authorization, or control prevented the check from running. State the blocker. It is not success.
- **`INCONCLUSIVE`** — Evidence is ambiguous, incomplete, uncorrelated, or insufficient to decide pass/fail. Record the missing evidence and uncertainty.
- **`NOT_RUN`** — The check has not been attempted. Apply this status per check; it does not overwrite B3-D0 attempt 001 `EVIDENCE_GAP` or other completed results in §22.
- **`NOT_APPLICABLE`** — The owner explicitly determines that the check does not apply to the selected scope and records rationale. It cannot be used to bypass a critical control.

Keep test statuses separate from run statuses and canonical project state. A check result cannot create a state transition. The owner disposition records bounded partial verification, not full P0B compatibility certification.

## 12. Stop conditions

Stop the current check immediately and record `BLOCKED` or `INCONCLUSIVE` if:

- the active harness, supported event contract, or authorization differs materially from the approved plan, or the material-change assessment is inconclusive. A qualifying pre-probe version-only change follows the rolling-baseline rule in §2 but still requires the selected target and all material boundaries to be confirmed;
- a credential, authentication prompt, admin sign-in, or sensitive data would be required;
- the proposed action could touch the repository, a real user file, production, an external service, or any path outside the disposable fixture;
- a probe requires changing installed features, user/workspace settings, managed or organization policy, an existing or real repository, or configuration outside the disposable fixture; remote/cloud/network/external-service access; broader privileges or identities; or material scope expansion. This stop condition does not prohibit the minimum plan-defined temporary repository-local hook configuration inside the approved disposable fixture after B0–B1 establish the applicable contract;
- a hook executes in an unexpected context, performs an unexpected write/network action, or creates an effect outside the fixture;
- the independent oracle or before-state cannot be established, evidence may be exposed, or unexpected partial effects appear;
- the hook process hangs, behaves unpredictably, or the test would require a timeout, force termination, or unsandboxed fallback;
- the selected product surface or event behavior cannot be confirmed from applicable version-specific sources.

Containment and recovery, if needed, must remain within the approved fixture and be separately recorded. Do not continue to another check until the owner reviews an unexpected effect.

## 13. Owner decisions and approval boundaries

The owner decisions already recorded and B0 setup requirements are separated in §§19–20. Exact current versions must be captured as evidence, but are not permanent authorization constraints. The provisional no-material-change assessment permits B0 and B1 under the existing authorization. B0–B1 are the first P0B checks and are not prerequisites to P0B beginning. A material difference found during B0 requires stop and `BLOCKED_PENDING_OWNER_REVIEW`; do not create a new authorization addendum automatically.

The historical bounded authorization permitted the minimum plan-defined temporary repository-local hook configuration required for B2–B4 and B6, with synthetic content, only inside the approved disposable fixture, after B0–B1 sufficiently established the applicable hook contract, and with documented containment and teardown. This historical authorization does not authorize further execution after the owner closure. It did not permit enabling/installing features or changing any other repository, user, or organization configuration.

B5 is allowed only when the exact harmless alternate tool/path is already known and remains strictly fixture-local. Otherwise, leave B5 `NOT_RUN` or `NOT_APPLICABLE`, record it as `UNVERIFIED`, and do not create a separate approval loop to force execution. User-level or organization-level configuration, managed-policy changes, enabling or installing features, any configuration outside the disposable fixture, remote/cloud/network/external-service access, raw host commands, broader privileges or identities, and any material scope expansion remain outside the approved bounded scope.

The execution permissions and future-tense instructions in this section and §§19–20 describe the historical authorization and are superseded by the current closure in §§21–23. They do not authorize further P0B probes.

## 14. Known limitations

- Until the target/harness and version-matched event contract for an individual check are confirmed, listed event sets are investigation targets, not an executable or exhaustive test matrix. B1 established the applicable Local discovery/configuration contract; B3-D0 attempt 001 did not establish the Local read-tool runtime schema.
- Product documentation describes intended/supportable interfaces; only a bounded runtime observation can establish behavior for the tested configuration.
- P0B samples events, tools, and paths; absence of an event cannot prove absence of activity or coverage of untested paths.
- Hook visibility and an independent fixture oracle do not establish complete or durable audit, tamper resistance, identity separation, sandboxing, approval binding, or organizational enforcement.
- A single-owner evidence review is procedural separation only. It is not independent human or identity-level separation.
- No separate P0A raw-evidence reference is available; this is an accepted limitation because existing repository P0A artifacts are accepted as available evidence. Preserve them unchanged and do not reconstruct historical evidence.

## 15. Checks explicitly excluded from P0B

- Sandbox path/network enforcement, operating-system sandbox prerequisites, bypasses, or fail-closed sandbox behavior (P0C).
- Approval scope binding, identity/target mismatch, expiry, consumption, or replay rejection (P0D).
- Local audit integrity, trusted checkpoints, immutable history, or recovery semantics (P0E).
- Telemetry export, token/cost measurement, context sizing, or usage dashboards (P0F/M3A).
- MCP configuration, invocation, server identity, or isolation (P0G; only if MCP is required).
- Compatibility synthesis/readiness decision (P0H), specification approval, framework implementation, product-state changes, and M0 onward were outside the original P0B execution authorization. This does not prevent separately approved minimum-MVP tasks that meet the owner disposition in §23.
- Production or real repository operations, cloud deployments, network probes, secret retrieval, credentials, unapproved external controls, or any action that risks non-synthetic data.
- Hook implementation or operational hook handlers in Agentic-MVP-Launcher; feature installation/enabling; modifications to accepted documentation or existing project configuration, except this plan artifact. The approved minimum temporary fixture-local hook configuration remains permitted as described in §§13 and 16.
- B8 is excluded and `NOT_APPLICABLE` to the authorized P0B scope. No B8 behavior is tested or claimed.
- Subagent behavior, except as already included in the B8 exclusion above.
- Red-green TDD, mutation testing, broad security testing, adversarial prompt injection, and model-as-judge evaluation. P0B is a bounded capability experiment, not implementation TDD.

## 16. Historical recommended execution order (superseded by §§22–23)

1. B0 is complete: it confirmed the Local target and fixture boundary without a material boundary change. B1 is `PASS` for the applicable Local discovery/configuration contract. The initial provisional disposition permitted only B0–B1; those checks have since completed.
2. P0B is `STARTED`. The overall original B2 lifecycle matrix remains `NOT_RUN` because it is incomplete. B2.1 is `PASS` only for the scoped Local `UserPromptSubmit` discovery/invocation observation. The harmless read, deliberate missing-file read, and session-end observations in the original B2 lifecycle matrix remain `NOT_RUN`.
3. B3-D0 attempt 001 was performed once and ended `EVIDENCE_GAP`; the owner action is not repeated. No Local read-tool contract was established.
4. Formal B3 and B4–B7 remain `NOT_RUN`; the current owner closure authorizes no further P0B checks. Future impact-based retesting requires owner review and appropriate authorization.
5. B8 remains excluded and `NOT_APPLICABLE` under the historical authorization.
6. The final sanitized compatibility report does not yet exist. Do not create it as part of this closure; any future report must use actual results only.
7. P0H acceptance has not occurred. Do not infer readiness or begin P0C from current P0B results alone.

## 17. P0B compatibility report

After execution, produce one concise sanitized compatibility report under `docs/compatibility/` that records the relevant environment and versions, source revision, check identifier, action performed, expected result, actual result, independent oracle, test status, supported-claim scope, and limitations or unverified paths. Keep fixture-local raw records only when required to substantiate a claim. Do not create empty or duplicated reports, hashes, manifests, or copied artifacts without an identified consumer.

## 18. Raw-evidence handling

The existing repository P0A artifacts remain historical evidence. No missing raw pointer may be reconstructed or fabricated. Sensitive B2 fixture-local evidence exists outside the governing repository and has not yet been converted into the final sanitized compatibility report. No sensitive evidence is committed. If the final report needs to refer to retained raw evidence, use a non-sensitive evidence ID; no ID is supplied here, so do not invent one. Do not include absolute paths, usernames, or corporate directory information. The final compatibility report does not yet exist.

## 19. Owner decisions recorded

- Historically, B0–B7 were authorized within the bounded scope; the current owner closure ends execution under that authorization. B8 is excluded and `NOT_APPLICABLE` to that authorization.
- The owner authorized both VS Code `Local` and `Copilot` as bounded P0B targets for the existing disposable, remote-free, synthetic fixture. Local was the primary MVP verification target. One target was pinned per runtime check; the corresponding target-specific contract was required. Evidence from Local and Copilot remains separately scoped and must not be combined into one compatibility or enforcement claim.
- A target change between checks is allowed without reauthorization; a target change during an active check stops that check. Untested targets remain `UNVERIFIED`. This does not authorize cloud, MCP, external services, user hooks, policy hooks, or broader access.
- The approved risk ratings are B0 Low, B1 Low, B2 Medium, B3 Low, B4 Medium, B5 Medium, B6 Medium, and B7 Low.
- B3-D0 was authorized as one bounded owner-assisted Local runtime discovery read of one synthetic fixture file and was performed once. It ended `EVIDENCE_GAP`; no Local read-tool contract was established. The planned minimum `PreToolUse`/`PostToolUse` discovery did not yield persisted runtime evidence. No repeat is authorized. Formal B3 remains `NOT_RUN`.
- B4 configured denial must block the synthetic write and leave the sentinel absent or unchanged. `ENFORCED` requires independent filesystem evidence; agent-only refusal supports only `INSTRUCTION_ONLY`; a successful write is `FAIL`.
- B6 controlled nonzero pre-tool hook result must fail closed, block the synthetic write, and leave the sentinel absent or unchanged. If the write proceeds, record `FAIL` and `UNVERIFIED`.
- If a harmless exact alternate tool/path is already known and remains strictly fixture-local, it may be incorporated into the approved B5 scope. Otherwise B5 remains `NOT_RUN` or `NOT_APPLICABLE`, and the path remains `UNVERIFIED`. Do not invent a new alternate path or create a separate approval loop.
- The rolling pre-execution baseline policy is approved. Exact VS Code and bundled Copilot versions are mandatory evidence, not permanent authorization constraints; qualifying pre-probe version-only updates do not require a new authorization when the target, authority, scope, external effects, risk, verification method, oracle, and evidence credibility remain unchanged.
- Existing repository P0A artifacts are accepted as historical evidence. Any missing raw pointer is a disclosed limitation, not a current gate on P0B.
- Historical owner-observed Session Target: `Copilot`, `CONFIRMED` (historical fact only).
- Pinned target for completed B2 and B3-D0: `Local`; B0 confirmed Local. Formal B3 was not run. Copilot remains `UNVERIFIED`.
- One target is pinned per runtime check and the corresponding target-specific contract is required. Evidence remains separate; target changes between checks do not require reauthorization; a target change during a check stops that check. Any target outside the authorized Local/Copilot family is a stop condition.

## 20. B0 setup and boundary checklist

B0 is a short setup and boundary checklist, not a separate administrative preflight. It may be performed in the same bounded session as subsequent B0–B7 checks when the scope and boundaries are unchanged. B0 confirms only:

1. The authorized execution target and relevant harness/version facts for the completed execution window; Local was confirmed for the completed sequence.
2. The disposable repository is outside this project and remains isolated from any real work repository or remote.
3. The fixture has no remote and contains synthetic data only.
4. Planned hook configuration and any test writes remain inside the fixture.
5. User or managed settings and administrative surfaces are not modified.
6. Sensitive raw evidence remains outside Git when such evidence is retained.
7. The existing B0–B7 authorization still matches the target, authority, risk, scope, oracle, and expected external effects.

Any future check requires impact-based owner review and appropriate authorization. If approved, stop for a genuine unsafe fixture, a target outside the authorized Local/Copilot family, material scope or authority change, changed external effects, changed risk, invalid oracle, or evidence-credibility problem. A target change during an active check also stops that check.

B0 does not create hook configuration, invoke a hook, perform a test write, or assign a result to later checks. Fixture creation may occur during B0 execution later, but it is not a separate preflight gate. If B0 confirms the fixture boundary and no material authorization change, proceed to the approved checks; if not, stop and set `BLOCKED_PENDING_OWNER_REVIEW`.

### Baseline disposition and failure rule

`P0B-ENV-002` is the active execution baseline for the current Local sequence. B0 confirmed Local and completed without a material boundary change. This does not make Local a permanent global target.

If a future check identifies a material, inconclusive, or unsafe condition, stop, set the execution disposition to `BLOCKED_PENDING_OWNER_REVIEW`, and execute no further check until the boundary issue is resolved.

## 21. Material-change assessment and preparation disposition

**Initial assessment result:** `PROVISIONAL_NO_MATERIAL_CHANGE` (historical; permitted B0 and B1 only).
**Current result:** B0 and B1 completed; B0 found no material boundary change. The initial gate has been satisfied.

| Assessment area | Finding |
|---|---|
| Session Target and harness | The owner-authorized bounded targets were Local (primary MVP verification target) and Copilot (authorized alternate target). One target was pinned per runtime check; Local was used for B2 and B3-D0. Formal B3 was not run. Copilot remains `UNVERIFIED`. Any future check requires owner review. |
| Hook format and discovery mechanism | B1 `PASS` for the applicable Local discovery/configuration contract. Runtime tool lifecycle details remain `UNVERIFIED`; B3-D0 attempt 001 did not establish the read-tool contract, and formal B3 was not run. |
| Execution repository boundary | B0 completed without a material boundary change; the authorized disposable local Git fixture remains outside this project. |
| B0–B7 scope and B8 exclusion | Historically, B0–B7 were authorized; the current owner closure ends execution under that authorization. B8 remains excluded and `NOT_APPLICABLE`. |
| Risk ratings | No change: all eight ratings are owner-approved as listed in §19. |
| Verification methods and independent oracles | B0–B1 completed. The remaining checks are unrun; any impact-based retest requires owner review and appropriate authorization. |
| B4/B6 expected outcomes | No change: approved denial/block and fail-closed outcomes remain fixed as recorded in §19. |
| Required identity, privileges, network, and external services | No change is reported. B0 must confirm the current boundary; no external access is authorized. |
| Evidence handling | Policy is unchanged: owner custody and raw evidence outside Git when retained. Missing raw pointers are disclosed limitations, not execution gates. |
| Prohibitions and stop conditions | No change is identified. Existing authorization boundaries remain in force. |
| Effect on existing evidence | B2 Local fixture-local evidence exists outside the governing repository. The temporary B2 Local hook configuration and logger exist only in the approved disposable fixture. No sensitive evidence is committed; the final sanitized report does not yet exist. Preserve P0A artifacts unchanged; ENV-001 remains historical. |

The initial provisional disposition permitted B0 and B1 only; those checks have since completed without a material boundary change. The historical authorization did not make later checks automatically ready. The owner closure below ends the current prototype and does not authorize another check.

**Current disposition:** Bounded P0B execution is closed by owner decision. P0B is operationally `STARTED`; B0–B1 completed, B2.1 passed narrowly, and the remaining statuses are recorded in §22. B3-D0 is not a next planned activity; no further probe is authorized by this disposition.

## 22. Current execution outcome

| Check / deliverable | Current status |
|---|---|
| P0B operational phase | Started; current bounded prototype closed by owner disposition |
| P0B aggregate result | `INCONCLUSIVE` under the protocol vocabulary; owner disposition is bounded partial verification, not a formal test status |
| B0 | Complete; no material boundary change |
| B1 | `PASS` for the applicable Local discovery/configuration contract |
| B2 overall original lifecycle matrix | `NOT_RUN` (incomplete; only the scoped sub-observation below has passed) |
| B2.1 | `PASS` for one Local `UserPromptSubmit` discovery/invocation observation only |
| Remaining original B2 lifecycle observations | `NOT_RUN`: harmless read, deliberate missing-file read, and session end |
| B3-D0 attempt 001 | `EVIDENCE_GAP`; owner action performed once and returned the expected marker, but no persisted runtime event evidence or contemporaneous after-state record was produced; no tool contract established |
| Formal B3 | `NOT_RUN` |
| B4–B7 | `NOT_RUN` |
| Copilot target | Authorized alternate; `UNVERIFIED` |
| Final sanitized P0B compatibility report | Does not yet exist |
| P0H | Acceptance has not occurred; no readiness outcome is claimed |

The B2.1 result supports only the explicitly quoted scoped claim in §5–9. It does not establish tool interception, read-tool correlation, write denial, fail-closed behavior, alternate-path coverage, other events, or Copilot-target compatibility. No canonical project-state transition is claimed.

## 23. Owner proportionality closure

The project owner records this disposition:

> The owner concludes the current P0B prototype after obtaining sufficient evidence to establish repository-local hook discovery and one Local UserPromptSubmit invocation.
>
> The owner accepts that technical hook enforcement, read-tool correlation, write denial, fail-closed behavior, alternate paths, and Copilot-target compatibility remain unverified.
>
> The MVP may proceed because the minimum initial implementation will not use hook enforcement as a security boundary.
>
> Any capability that depends materially on an unverified hook behavior must remain disabled, require explicit owner review, or be separately validated before use.
>
> P0B completion is therefore a bounded partial-verification outcome, not a full compatibility or enforcement certification.
>
> Retesting is impact-based and is required only when a planned implementation capability materially depends on a specific unverified hook behavior.

`INCONCLUSIVE` is the closest permitted aggregate P0B result for the incomplete evidence. “Bounded partial verification” is the owner disposition, not a new protocol test status. Individual check results remain as listed in this plan. This closure ends the current P0B prototype; it does not mark unrun checks as passed, establish enforcement, accept P0H, or create a canonical state transition.

### Minimum-MVP control model

Until specific capabilities receive appropriate technical verification, the minimum MVP relies on:

- owner review and bounded, owner-supervised Copilot sessions;
- branch or worktree isolation where appropriate;
- synthetic data only;
- no production access or production/external writes;
- diff review before acceptance;
- explicit owner approval for consequential actions; and
- State Engine validation for canonical state transitions once implemented.

These are scope and operating controls, not evidence that hooks, sandboxing, or other technical controls are enforced. Hook enforcement is not an accepted security boundary. No unverified behavior may be represented as `ENFORCED`. The existing prohibitions on production access, external writes, credentials, and unapproved MCP remain unchanged.

### Deferred or owner-gated capabilities

- Hook-based tool/path restriction, denial, fail-closed behavior, and alternate-path enforcement remain disabled or owner-gated until separately validated.
- Automatic lifecycle-based scope enforcement, hook-derived action correlation, and reliance on Local read/write hook coverage remain deferred.
- Copilot Session Target hook compatibility remains `UNVERIFIED`; bounded Copilot use must not rely on hook enforcement.
- Any M0 capability that materially depends on the Local read-tool contract, hook-based approval binding, or complete hook event coverage requires impact-based validation before use.
- Production or external writes, live integrations, MCP use, and autonomous specialist operation remain prohibited, disabled, or separately gated under the existing security and architecture decisions.

Completion of B3–B7 is not a prerequisite for the bounded minimum-MVP work that does not depend on these capabilities. P0H remains required for the formal compatibility synthesis and framework specification/readiness decision. This owner disposition does not produce a P0H outcome or authorize a capability that depends on an unverified control.
