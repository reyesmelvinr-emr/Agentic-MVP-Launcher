# Implementation Plan / ADR Reconciliation

**Date:** 2026-10-20  
**Scope:** Documentation alignment only  
**Authority:** [GLOSSARY.md](../../GLOSSARY.md) and [ADRs 0001–0015](../adr/)

## Outcome

The implementation plan was reconciled with the accepted glossary and ADR decisions. ADRs 0001 through 0015 were confirmed to exist and were read before editing. The ADRs and glossary were not changed. No framework implementation, prototype, or P0 work was started.

## Contradictions and material gaps found

Two plan statements directly conflicted with accepted decisions:

- The §17 exit criterion “Specialists cannot access undeclared tools/paths/targets” treated a declaration as proof of enforcement. [ADR 0005](../adr/0005-verified-enforcement-boundaries.md) requires verified supported controls and an effective independent control for consequential actions. The exit criterion now requires that evidence.
- The §5.4 phrase “where available” made a trusted checkpoint optional for history verification. [ADR 0014](../adr/0014-tamper-evident-event-history.md) permits a tamper-evidence claim only relative to a trusted checkpoint. The plan now calls history without one unverified.

Other decisions were missing or ambiguous rather than directly contradicted: sole-writer and stale-version rules, task-packet authority/version binding, verified enforcement scope, approved least-privilege identity topology, exact one-time approval binding, MCP enablement requirements, telemetry limits/order, core/profile/extension and risk boundaries, reviewed framework upgrades, and full run-status/recovery semantics. They are reconciled below.

## Reconciled plan areas

### Durable state and transitions (§5.2–5.4, §6.1, §9, §18)

Made the transition log canonical, the projection rebuildable, and the State Engine the sole writer. Coordinator/agent transition requests carry an expected state version; stale requests require explicit refresh. Invalid history is quarantined; recovery does not replay external actions.

Governing decisions: [ADR 0001](../adr/0001-event-log-as-state-source.md), [ADR 0002](../adr/0002-single-writer-state-transitions.md).

### Coordinator and specialist invocation (§6.1, §14)

Preserved recommendation-only Coordinator behavior and operator-directed specialist invocation; required deterministic completion evidence for phase-transition requests and a harness prototype before any future direct specialist invocation.

Governing decisions: [ADR 0002](../adr/0002-single-writer-state-transitions.md), [ADR 0003](../adr/0003-coordinator-boundary.md).

### Task packets and context (§8.1–8.3, §9)

Required immutable packets bound to the state version, governing profiles/policies, authoritative context, exact scope, and completion criteria. Material changes require a new packet; summaries remain advisory and raw logs are untrusted/excluded by default.

Governing decision: [ADR 0004](../adr/0004-version-bound-task-context.md).

### Enforcement boundaries (§7, §9, §17)

Replaced an unconditional access-control claim with verified-control requirements; consequential actions require an effective independent control and are unavailable if it cannot be verified.

Governing decision: [ADR 0005](../adr/0005-verified-enforcement-boundaries.md).

### Integration identities (§7, §14 M0C-B/M2B)

Required organization approval of actual identity topology, least privilege, meaningful read/write separation, protected pipelines for writes, and disabling actions that cannot be constrained to approved scope.

Governing decision: [ADR 0006](../adr/0006-least-privilege-integration-identities.md).

### Approvals (§8.3, §9)

Bound each approval to one exact execution scope, task packet, manifest, state version, operator, repository/branch, paths, tools/actions, targets/domains, and expiry. Scope changes require a new approval; approval cannot be transferred or reused.

Governing decision: [ADR 0007](../adr/0007-one-time-execution-approval.md).

### MCP (§10, §14 M0C-B)

Kept MCP separate from terminal sandboxing and specified that it remains disabled until its source/version, tools, identity, credentials, network, logging, read/write separation, and controls are reviewed and verified.

Governing decision: [ADR 0008](../adr/0008-independent-mcp-boundary.md).

### Telemetry and audit (§12, §14 M0C-A/M3A)

Limited telemetry to provider-/harness-exposed facts; labeled estimates/unavailable values; made prompt/tool-output capture off by default and export subject to organizational approval. Kept local audit ahead of provider telemetry and provider proof in M3A.

Governing decisions: [ADR 0009](../adr/0009-provider-usage-telemetry.md), [ADR 0010](../adr/0010-audit-before-usage-telemetry.md).

### Core, profile, extension, and risk boundaries (§2, §5.1)

Defined the project-independent core, project-specific profile, and distinct project extensions. Made risk profiles additive-only and non-weakenable against the baseline.

Governing decisions: [ADR 0011](../adr/0011-framework-core-and-extensions.md), [ADR 0012](../adr/0012-risk-profiles-cannot-weaken-baseline.md).

### Framework versions (§5.1, §18)

Added a pinned framework version and required reviewed upgrades/migrations with compatibility and security-impact information, customization preservation, and an urgent-fix path.

Governing decision: [ADR 0013](../adr/0013-reviewed-framework-upgrades.md).

### Event-history integrity (§5.4, §14 M3A)

Required a trusted checkpoint for a tamper-evidence claim; history without one is unverified. Invalid history is quarantined; recovery does not replay external actions, and baseline protection depends on the State Engine and repository controls. Did not imply immutability or require stronger signed/external storage absent a threat-model need.

Governing decisions: [ADR 0002](../adr/0002-single-writer-state-transitions.md), [ADR 0014](../adr/0014-tamper-evident-event-history.md).

### Run status and recovery (§9)

Defined `blocked`, `failed`, `needs-review`, and `rolled-back`; required operator authorization to resume after review; required a new run for changed scope or invalid history; prohibited automatic external-action retries or replay.

Governing decisions: [ADR 0002](../adr/0002-single-writer-state-transitions.md), [ADR 0015](../adr/0015-run-status-and-recovery-semantics.md).

## Decisions retained without behavioral change

- The append-only transition log remains canonical and the state projection remains rebuildable.
- The Coordinator recommends; the operator starts a specialist in the initial MVP. Autonomous specialist routing is not introduced.
- M0C remains split into M0C-A (enforcement/local audit/approval) and M0C-B (sandbox/identity/MCP boundary).
- M1B remains an optional, separately approved read-only subagent pilot.
- The milestone sequence and general architecture remain intact.
- Local audit is required before specialist work. Provider telemetry proof is deferred to M3A and is not a prerequisite for read-only fixture work.

## Completion boundary

This reconciliation changes documentation only. It does not implement, prototype, or initiate any framework component or later implementation/P0 work. See the [handoff](../handoffs/PLAN-RECONCILIATION-handoff.md) for files changed and validation.
