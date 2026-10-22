# Agentic MVP Launcher

Agentic MVP Launcher is a reusable framework design for planning and
implementing small-to-medium MVP projects using GitHub Copilot and Visual
Studio Code. Its target architecture is a Supervised Agent Graph with Durable
Project State. The framework is not yet implemented. The current P0B prototype
has a bounded partial-verification disposition; formal framework readiness
remains unaccepted.

## Current Status

- Architecture review completed
- Glossary and ADRs 0001 through 0016 recorded
- Implementation plan reconciled with accepted ADRs
- P0A environment and harness inventory complete
- T0 documentation baseline accepted
- P0B started and closed by owner disposition as bounded partial verification (`INCONCLUSIVE` aggregate); no full compatibility or enforcement certification
- P0B exact check results: B0 complete without material boundary change; B1 `PASS`; B2.1 `PASS` narrowly for one Local `UserPromptSubmit` observation; B3-D0 `EVIDENCE_GAP`; formal B3 and B4–B7 `NOT_RUN`
- Copilot target remains `UNVERIFIED`; P0H acceptance has not occurred
- Separately approved minimum-MVP tasks that do not depend on unverified hook behavior may proceed under the documented owner-review and synthetic-operation controls; hook enforcement is not an accepted security boundary
- P0C–P0H verification not started
- Framework specification not started
- Framework implementation not started

The absence of a separate raw-evidence pointer remains a disclosed evidence limitation for P0A. See the [P0B verification plan](docs/testing/P0B-verification-plan.md) for the bounded outcome, remaining unverified capabilities, and impact-based retesting limits.

## Current Authoritative Planning Documents

- `Generic_Supervised_Agent_Graph_Implementation_Plan.md`
- `GLOSSARY.md`
- `docs/adr/`
- `docs/reviews/plan-adr-reconciliation.md`
- `docs/handoffs/PLAN-RECONCILIATION-handoff.md`
- `docs/reviews/T0_GRILL_SESSION_DECISIONS.md`
- `docs/reviews/T0_DOCUMENTATION_BASELINE_ACCEPTANCE.md`
- `docs/testing/p0-verification-protocol.md`
- `docs/adr/0016-verification-claims-and-acceptance-authority.md`

## Pre-Specification Gate

The P0 milestone verifies whether the selected Visual Studio Code and GitHub Copilot environment can enforce the controls described by the architecture.

P0 produces compatibility evidence. P0 does not implement the framework.

Sanitized P0 compatibility reports belong under `docs/compatibility/` as results are produced. Behavioral evidence and technical enforcement evidence are reported separately; an agent refusal does not prove that an action is blocked. Keep sensitive raw evidence outside Git in an owner-controlled or approved restricted location, referenced only by a non-sensitive evidence ID.

## Examples

Sanitized example projects are stored under `examples/`.

## Security

Do not commit credentials, raw enterprise logs, production data, or unredacted evidence. See `SECURITY.md`.
