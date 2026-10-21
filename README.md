# Agentic MVP Launcher

Agentic MVP Launcher is a reusable framework design for planning and
implementing small-to-medium MVP projects using GitHub Copilot and Visual
Studio Code. Its target architecture is a Supervised Agent Graph with Durable
Project State. The framework is not yet implemented, and the current
repository is in pre-specification compatibility verification.

## Current Status

- Architecture review completed
- Glossary and ADRs 0001 through 0016 recorded
- Implementation plan reconciled with accepted ADRs
- P0A environment and harness inventory complete
- T0 documentation baseline accepted; P0B remains not started under the current bounded B0–B7 authorization
- P0B–P0H verification not started
- Framework specification not started
- Framework implementation not started

The absence of a separate raw-evidence pointer remains a disclosed evidence limitation for P0A, but it does not by itself block P0B. B0 independently verifies the current conditions before any probe.

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
