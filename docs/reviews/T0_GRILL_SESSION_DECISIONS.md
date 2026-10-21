# T0 Grill Session Decisions

**Date:** 2026-10-21
**Authority:** Agreed grill-session summary supplied for this documentation task
**Status:** Decisions recorded; T0 is approved but not complete.

## Current Project State

- P0A is complete. Preserve all historical P0A evidence unchanged.
- T0 is an approved, documentation-only readiness workstream required before substantive P0B verification. T0 is not a project-state milestone and does not advance canonical project state.
- P0B through P0H and M0 onward have not started.
- The append-only transition log remains canonical; the state projection is rebuildable; the State Engine remains the sole writer of state transitions and the projection.
- This record does not mark T0 complete or change project state.

## A. Agreed Decisions

- T0 is a documentation-only readiness workstream after P0A, required before P0B. It does not renumber P0B–P0H or advance project state; P0A evidence is preserved and no new phase is marked complete.
- Minimum T0: one normative P0 protocol with method selection, statuses, evidence template, claim labels, review/gate rules, and lightweight M0 guidance. No schemas, machine-enforced gates, or full test catalog now.
- Implementation tasks bind the approved seam, oracle, method, acceptance threshold, and evidence in the immutable task packet. Material changes require a revised packet; execution-neutral updates may be recorded as addenda. P0 probes use the protocol, not implementation packets.
- Test statuses are `PASS`, `FAIL`, `BLOCKED`, `INCONCLUSIVE`, `NOT_RUN`, and `NOT_APPLICABLE`. Only `PASS` satisfies a required check. `NOT_APPLICABLE` needs owner rationale; no non-pass status implies success.
- P0H outcomes are `READY_FOR_SPEC`, `READY_FOR_SPEC_WITH_EXTERNAL_CONTROLS`, `NOT_READY_CRITICAL_CONTROL_GAP`, and `NOT_READY_INCOMPLETE_EVIDENCE`. Only the first two permit specification work.
- Behavior and enforcement are reported separately. P0 findings classify controls as `ENFORCED`, `EXTERNAL_CONTROL`, `INSTRUCTION_ONLY`, or `UNVERIFIED`, with evidence and limitations.
- Critical enforcement checks use applicable allow, deny, bypass, fail-closed, evidence, and partial-effect recovery tests. Mutation testing waits for deterministic implementation controls.
- TDD is selective: intended-reason red evidence applies only when an approved task chooses TDD for deterministic behavior with an approved seam and independent oracle. P0 uses documented experiments, expected-versus-actual results, compatibility evidence, and repeatability checks.
- Expected results come from reviewed requirements, independent fixtures/calculations, schemas, contracts, or invariants, never the production logic under test or a model judging itself.
- Material reviews report four separate axes: specification, standards, security, and test/evidence sufficiency. Before review agents exist, the owner/operator performs a separate review step and explicitly accepts P0H; this is procedural, not independent human or identity separation.
- Verified external controls are mitigations, not waivers. A waiver cannot clear a critical P0 gap or permit specification; only closure or a verified external control can. Expired waivers block readiness.
- Compatibility retesting is impact-based, with tested versions/configuration, impact assessment, selected checks, and exclusions recorded. Broad or unknown impact triggers full recertification.
- Keep the M0A build prompt unchanged until P0H and the specification are approved. No new privileged testing agent is proposed.

## B. Unresolved Owner Inputs

No policy decisions remain open. Before relevant P0 checks, the owner must confirm the selected harness and versions, approved restricted-evidence location, whether MCP is required, risk classifications for individual checks, and where P0A inventory evidence is preserved. The repository tree reviewed here has no visible P0A evidence artifact; that does not establish that evidence is missing elsewhere.

## C. Risks and Gaps

- No direct conflict with ADRs 0001–0015. The plan already has test requirements, but they are distributed across task packets, hooks, sandboxing, milestones, dashboards, and exit criteria.
- The plan lacks shared status, evidence, traceability, and P0H rules. It also does not name a home for sanitized compatibility reports.
- Preserve the distinction between test status and existing run statuses (`blocked`, `failed`, `needs-review`, `rolled-back`); individual test records must not become state-transition events.
- P0 evidence must not imply hook, sandbox, approval, telemetry, MCP, identity, or model capabilities beyond what a test verifies. A verified external control must be explicit and carried into the specification.
- The existing reconciliation handoff says it did not start P0 or implementation; this is consistent with P0A complete and P0B pending. The example Wiz materials contain useful security-testing patterns but remain project-specific, not framework authority.

## D. Glossary

Add concise definitions for `test seam`, `independent oracle`, `verification evidence`, `behavioral evidence`, and `enforcement evidence`. Keep status values and procedures in the protocol, explicitly distinguishing test statuses from run statuses in the glossary or protocol.

## E. ADRs

Add one narrow ADR, provisionally `0016-verification-claims-and-acceptance-authority.md`, for behavioral/enforcement claim separation, prohibition on implementation self-certification, owner acceptance for this single-owner MVP, and the State Engine’s sole authority to record transitions. Reference ADRs 0002, 0004, and 0005. No existing ADR amendment is required unless drafting reveals a wording conflict; do not turn test procedures into ADRs.

## F. Implementation Plan Changes

- Front matter: increment version/date only during the approved edit; summarize the testing-baseline change.
- §1: add verification as a cross-cutting capability tied to requirements, risk, observable behavior, and credible evidence.
- §2: add fit-for-purpose method selection and independent-oracle / behavioral-versus-enforcement principles.
- §3: show a Verification Plane for traceability and evidence that grants no authority and cannot advance state.
- §4: point to `docs/testing/` and future sanitized P0 reports under `docs/compatibility/`; keep executable tests and fixtures under `tests/` when needed.
- §6–7: assign planning, implementation, validation, and review duties to existing roles; do not add a testing agent or expand capabilities. Implementers cannot alter their own gates or self-approve.
- §8: bind material implementation verification details to immutable task packets; keep P0 probes under the P0 protocol.
- §9: evidence records reference requirements, test/run/packet IDs, versions, result, and evidence location; they do not alter the canonical event-log model. Hooks count only where coverage is verified.
- §10: classify required sandbox checks using the agreed risk-based allow/deny/bypass/fail-closed/evidence/recovery rule.
- Add a concise verification section after §10 for method selection, status meanings, evidence, reviews, waivers, and gate rules; do not reproduce the full protocol.
- §§11–13: keep reporting deterministic; defer dashboards and broad testing-cost infrastructure until supported by implementation evidence.
- §14: insert T0 after P0A as a readiness workstream before P0B, not a new project-state milestone. Add the four P0H outcomes and the agreed phase-specific evidence map.
- §§15–18: require separate validation/review for material acceptance, add critical-control verification exit criteria, and record false-red, tautology, test-weakening, and self-certification risks.
- §19: leave the M0A prompt unchanged until P0H and specification approval; then require packet-bound methods, seams, oracles, evidence, and separate review results.
- §20: record versions and retest triggers for selected product references; do not imply unverified capabilities.

## G. Other Documentation

- `README.md`: link the protocol and compatibility evidence location; preserve current phase status.
- `SECURITY.md`: clarify sanitized versus restricted raw evidence and prohibit unsupported enforcement claims.
- `docs/compatibility/`: create sanitized P0 reports as evidence is produced; do not fabricate empty reports or retroactively claim capability.
- `docs/governance/`: do not create a new governance system now; the protocol and narrow ADR cover the agreed rules.
- `plan-adr-reconciliation.md`: leave the historical reconciliation intact; future review records use four axes and disclose procedural independence limits.
- Evidence: keep sensitive raw evidence outside Git in owner-controlled or approved restricted storage; reference it by non-sensitive evidence ID when needed.
- Prototypes: allow only bounded P0 probes where necessary; no operational implementation or live integrations.
- Tests: no executable suite exists yet. Add synthetic/sanitized fixtures under `tests/fixtures/` when implementation testing requires them; never optimize for test count.
- Do not modify the Wiz example documents as though they govern the generic framework.

## H. Minimum Artifact Tree

```text
docs/
  adr/0016-verification-claims-and-acceptance-authority.md
  testing/p0-verification-protocol.md
  compatibility/                 # P0 reports as checks are run, not a T0 placeholder
GLOSSARY.md
Generic_Supervised_Agent_Graph_Implementation_Plan.md
README.md
SECURITY.md
```

No `project-control/testing/`, schemas, or machine-readable gates in the initial baseline.

## I. Documentation-Only Sequence

1. Reconfirm P0A evidence location and preserve it unchanged.
2. Draft the narrow ADR and glossary terms; check against ADRs 0001–0015.
3. Write the P0 protocol and evidence template, including status and claim labels.
4. Update the plan and README/SECURITY references without changing phase status.
5. Review four axes, verify links/terminology, then record a documentation-only handoff.
6. Start P0B only after T0 approval; create compatibility reports as evidence is collected.

## J. Deferred

Defer machine-enforced quality gates, broad schemas/catalogs, specialized testing agents, unrestricted test generation, mutation testing before deterministic controls exist, broad metamorphic/repeated-model evaluation, model-as-judge acceptance, cross-model benchmarking, general chaos engineering, and new evidence infrastructure/service identities.

## K. Recommended Next-Session Prompt

```text
Apply the approved documentation-only T0 decisions from the grill session.
Read the implementation plan, GLOSSARY.md, README.md, SECURITY.md, ADRs 0001–0015, and the reconciliation review/handoff first.
Update the plan and glossary; add the narrow verification/acceptance ADR and docs/testing/p0-verification-protocol.md.
Update README.md and SECURITY.md only as needed to reflect authority and evidence locations.
Do not modify historical P0A evidence, start P0B, create operational agents or code, or mark T0/P0B–P0H/M0 complete.
Keep raw evidence outside Git, preserve the State Engine as sole state writer, and do not claim unverified enforcement.
Finish with a documentation-only diff review, link/terminology validation, and handoff.
```
