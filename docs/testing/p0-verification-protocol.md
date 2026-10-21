# P0 Verification Protocol

**Status:** Normative for P0 verification after owner approval of the T0 documentation baseline.<br>
**Scope:** Evidence rules for P0A–P0H and lightweight guidance for later implementation work.<br>
**Authority:** This protocol defines verification and evidence expectations. It does not implement controls, establish product capability, authorize external actions, or advance canonical project state.

## 1. Purpose and Boundaries

P0 determines what the selected Visual Studio Code and GitHub Copilot environment, plus any explicitly approved external controls, can demonstrate. It does not implement the framework. P0A is complete; preserve its historical evidence unchanged. T0 is a documentation-only readiness workstream, not a project-state milestone. P0B is `AUTHORIZED_FOR_BOUNDED_EXECUTION` and may be performed in one bounded session; B0 begins the execution and includes a short setup and boundary checklist. P0C–P0H and M0 onward remain not started.

The append-only transition log remains the canonical state record, the state projection is rebuildable, and the State Engine remains the sole writer of both. Test records and review results are evidence; they are not state-transition events. A Coordinator, implementation agent, or model may prepare findings but cannot accept readiness or directly change state.

No behavior, hook coverage, sandbox boundary, approval enforcement, telemetry signal, MCP isolation, identity restriction, or model capability is assumed. Each claim is limited to the environment, configuration, version, action, and evidence actually examined. An untested path or uncovered event remains unverified.

## 2. Test Statuses

Use these statuses for individual checks:

- `PASS`: The expected result was observed and supported by sufficient evidence.
- `FAIL`: The expected result was not observed and effects are understood.
- `BLOCKED`: A prerequisite or control prevented execution.
- `INCONCLUSIVE`: Available evidence cannot establish pass or fail.
- `NOT_RUN`: The check has not been executed.
- `NOT_APPLICABLE`: The check does not apply, with a documented project-owner rationale.

Only `PASS` satisfies an applicable required check. `BLOCKED`, `INCONCLUSIVE`, and `NOT_RUN` never imply success. `NOT_APPLICABLE` cannot be used to bypass a critical control.

Test statuses are distinct from run statuses. Run statuses retain the meanings in ADR 0015: `blocked`, `failed`, `needs-review`, and `rolled-back`. A check result does not by itself determine or write a run or project status.

## 3. Verification Claim Classifications

Report behavioral evidence separately from technical enforcement evidence. Classify each control claim as one of:

- `ENFORCED`: A tested control in the selected harness or execution environment was verified to block the prohibited action within the tested scope; the claim is not being reported as a separately governed external control.
- `EXTERNAL_CONTROL`: A separately governed control outside the selected harness boundary was identified and verified. Record its owner, scope, limitations, and evidence; carry an accepted control forward as a specification requirement.
- `INSTRUCTION_ONLY`: Evidence shows instruction-following behavior but does not establish that the action is technically blocked.
- `UNVERIFIED`: Evidence is absent, incomplete, inconclusive, blocked, or outside verified coverage.

An agent refusal or compliant response may support `INSTRUCTION_ONLY`; it never proves `ENFORCED`. A product capability observation is not an enforcement claim. Do not label an external control as a framework control, or claim protection beyond tested scope. These boundaries follow ADR 0005 and ADR 0016.

## 4. Selecting Verification Methods

Select methods for the observed behavior, consequence of failure, stable seam, independent oracle, and available evidence. Record why the chosen method is credible and which important defects it cannot detect. Use the least costly method that credibly answers the requirement; do not optimize for test count or require every method.

P0 is compatibility verification, not framework implementation testing. Use bounded experiments, documented steps, expected-versus-actual results, version/configuration records, and repeatability checks where useful. TDD is not a blanket P0 requirement.

For critical enforcement controls, require all applicable checks from this set: permitted action, prohibited action, relevant bypass, fail-closed behavior, evidence correlation, and recovery where partial effects are possible. Document any inapplicable check with an owner rationale. For other high-risk behavior, use positive and negative cases, malformed or boundary inputs, recovery where relevant, and durable regression coverage when implementation exists. Lower-risk deterministic behavior may use examples, schema/contract checks, invariants, or deterministic regeneration where those methods establish the requirement.

Mutation testing is deferred until deterministic enforcement components exist and is then limited to critical controls where it provides meaningful assurance. Detailed metamorphic, repeated-model, and agent-evaluation infrastructure is deferred unless a demonstrated risk justifies it.

## 5. P0 Phase Evidence

### P0A: Environment and Harness Inventory

Record the environment and harness inventory, tested versions/configuration, and evidence references. P0A is complete; do not rewrite or replace its historical evidence. If a separate raw-evidence pointer is not available, record that as a disclosed evidence limitation and continue; do not reconstruct or fabricate missing evidence. B0 independently verifies current conditions. Inventory alone does not prove enforcement.

### P0B: Tool and Hook Coverage

Record which tested lifecycle events and tools traverse the selected harness and the conditions under which coverage was observed. Where enforcement is claimed, test allowed and denied actions and applicable bypass paths. Untested events, tools, or alternate paths remain `UNVERIFIED`; absence of a hook event is not proof that activity did not occur.

### P0C: Sandbox Enforcement

For the tested sandbox/configuration, record permitted and denied paths/actions, relevant bypass attempts, failure/unavailability behavior, and whether an unsandboxed fallback was prevented. Include partial-effect recovery evidence where relevant. A test result is limited to the tested host, runtime, policy, and tool paths.

### P0D: Approval Binding and Replay Rejection

Where supported, test scope binding, identity and target mismatch, expiry, consumption, and replay rejection. Record observed behavior separately from technical enforcement. Do not treat an approval prompt or instruction as proof that an action is technically blocked.

### P0E: Local Audit and Trusted Checkpoint Feasibility

Record which tested actions and outcomes appear in local audit evidence, its limitations, and trusted-checkpoint feasibility. A hash chain is tamper-evident only relative to a trusted checkpoint; do not claim immutability or prevention of complete history replacement. Keep local audit evidence distinct from provider-usage telemetry.

### P0F: Telemetry and Context-Sizing Feasibility

Record only provider- or harness-exposed telemetry and deterministic context-sizing observations, with unavailable values labeled unavailable and estimates labeled estimates. Do not infer exact cost or capability from absent signals. Follow ADRs 0009 and 0010 for telemetry limits, approval, and ordering.

### P0G: MCP Boundary Verification

Run only if MCP is required for the MVP. Assess the specific server/tool source and version, identity, credentials, network, logging, read/write separation, and relevant controls as an independent boundary. Terminal sandbox results do not establish MCP isolation. If MCP is not required, record the owner rationale and leave MCP disabled; do not simulate a live MCP capability.

### P0H: Compatibility Synthesis and Readiness

Synthesize phase evidence, limitations, unresolved findings, external controls, and the four review axes. The only allowed outcomes are:

- `READY_FOR_SPEC`
- `READY_FOR_SPEC_WITH_EXTERNAL_CONTROLS`
- `NOT_READY_CRITICAL_CONTROL_GAP`
- `NOT_READY_INCOMPLETE_EVIDENCE`

Only the first two permit specification work. A critical gap permits `READY_FOR_SPEC_WITH_EXTERNAL_CONTROLS` only when an external or compensating control is explicit, verified, evidenced, accepted by the project owner, and carried forward as a binding specification requirement. A waiver alone never clears a critical gap or permits specification. Unresolved mandatory review findings map to the applicable not-ready outcome.

## 6. Minimum Evidence Record

Use this lightweight record for each material check. P0 evidence may be a sanitized report or a reference to approved restricted raw evidence; never commit credentials, authentication caches, or unnecessary sensitive raw evidence.

```yaml
check_id: <stable-id>
requirement_or_control: <id-or-description>
p0_phase: <P0A-P0H>
risk: <critical-high-medium-low>
method: <experiment-test-review-or-other>
claim_classification: <ENFORCED-EXTERNAL_CONTROL-INSTRUCTION_ONLY-UNVERIFIED>
environment:
  product_versions: <tested versions>
  configuration: <relevant non-sensitive configuration reference>
preconditions: <fixture and conditions>
steps: <reproducible steps or command>
independent_oracle: <reviewed requirement-fixture-schema-contract-invariant>
expected: <expected observable result>
actual: <observed result>
status: <PASS-FAIL-BLOCKED-INCONCLUSIVE-NOT_RUN-NOT_APPLICABLE>
evidence_reference: <sanitized path or non-sensitive restricted-evidence ID>
limitations: <coverage gaps and uncertainty>
actor: <operator or agent/session reference>
timestamp: <timestamp>
```

The evidence reference must not expose secrets or sensitive raw content. Preserve P0A records unchanged; later reports may reference them by their existing non-sensitive identifier if one exists.

## 7. Independent Oracles and Selective TDD

Expected results must derive from reviewed requirements, independently derived fixtures/calculations, schemas, contracts, or defined invariants. Never calculate expected output with the production function or equivalent logic under test, and never use a model to judge its own output as the sole acceptance oracle. Record the oracle for each check. Judgment-based outcomes require a documented rubric and separate project-owner or independent review.

Use red-green TDD only when an approved implementation task selects it for deterministic behavior with an approved public seam and independent oracle. The red phase is valid only when the check fails for the intended behavioral reason, not because of syntax, imports, missing helpers, invalid fixtures, dependencies, or placeholder APIs. Record intended and actual failure reasons where red-phase evidence is required. Do not obtain green by weakening, skipping, suppressing, or removing assertions, changing fixtures to normalize a defect, or replacing the independent oracle with production output.

For material implementation tasks, the immutable task packet records the approved seam, oracle, method, acceptance threshold, and required evidence before work starts. Material changes to scope, authority, control coverage, observed interface, expected behavior, threshold, or required evidence need a revised packet. Execution-neutral changes may be recorded as an addendum. This applies ADR 0004 without changing the P0 probe protocol.

## 8. Review and Acceptance

Material reviews report separate results, without averaging, for:

1. Specification conformance
2. Engineering and documentation standards
3. Security and authority boundaries
4. Test and evidence sufficiency

A mandatory-axis failure remains visible and blocks acceptance under the applicable gate. During the single-owner MVP, the project owner/operator performs a separate evidence-review step and records explicit P0H acceptance. This is procedural and session-level separation only, not independent human or identity-level separation. The implementer, Coordinator, and reviewing model may provide findings but cannot self-certify or accept readiness. See ADR 0016.

Acceptance does not directly alter canonical state. Only the State Engine validates and records an approved state transition.

## 9. External Controls, Waivers, and Gates

A verified external or compensating control is mitigation, not a waiver. Identify its owner, exact scope, limitations, evidence, and specification requirement. A critical failed, blocked, inconclusive, or unrun check blocks readiness unless the gap is closed or a verified external control meets the P0H criteria.

Record any waiver separately with owner, scope, rationale, residual risk, compensating controls, approval date, expiry, and retest trigger. A waiver alone cannot clear a critical gap or permit specification. An expired waiver blocks readiness until renewed, replaced, or closed. Only `PASS` satisfies a required check; incomplete evidence maps to `NOT_READY_INCOMPLETE_EVIDENCE` where applicable.

## 10. Compatibility Retesting

Retesting is impact-based. Rerun affected P0 checks when relevant selected harness/model, hook, sandbox, operating-system/runtime, approval, identity, MCP, policy, or configuration versions change. Record changed versions/configuration, impact assessment, selected checks/results, and why other checks were excluded. Broad, unknown, or cross-cutting impact requires full P0 recertification. Do not assume a product update preserves prior behavior without evidence.

For bounded P0 verification in the single-owner MVP, use a rolling environment baseline until the first probe. Exact VS Code and bundled Copilot versions remain mandatory evidence fields, but are not permanent authorization constraints. A version-only change before the first probe does not require a separate authorization addendum when all of the following remain unchanged: selected Session Target (`Copilot`); operating-system family and architecture; isolated disposable repository boundary; authorized B0–B7 scope; B8 exclusion; risk ratings; verification methods; independent oracles; expected B4 and B6 outcomes; evidence handling; prohibited actions; and stop conditions.

For a qualifying pre-execution version-only update, record the previous baseline as `SUPERSEDED_BEFORE_EXECUTION`, record the current environment as the active execution baseline, and recheck the execution window in B0 against that baseline. Preserve the existing bounded-execution authorization; do not require a separate addendum. This exception applies only before the first probe. Require owner review when an environment change materially affects the Session Target or harness; hook format or discovery mechanism; supported events or control surface; execution scope or repository boundary; required privileges, identity, network, or external services; risk, method, seam, oracle, or expected result; evidence credibility; or in-progress or completed test results. Owner review is also required when the material-change assessment is inconclusive. If the change cannot be established as version-only with all listed boundaries unchanged, stop for owner review.

Sanitized P0 reports belong under `docs/compatibility/` as reports are produced; do not create fabricated results or empty compatibility reports. Keep sensitive raw evidence in an owner-controlled or approved restricted location outside Git and refer to it only by a non-sensitive evidence ID.

## 11. Lightweight M0 Guidance

After P0H and specification approval, material M0 tasks should use the packet-bound method, seam, oracle, threshold, and evidence rules above. Prefer focused deterministic checks and durable regression coverage for implemented critical controls. Detailed M0 procedures, schemas, machine-enforced gates, a full test catalog, broad mutation testing, and agent-evaluation infrastructure remain deferred unless approved requirements or demonstrated risks justify them. Do not change the M0A build prompt before P0H and specification approval.
