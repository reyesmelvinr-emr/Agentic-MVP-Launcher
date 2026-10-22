# Generic Supervised Agent Graph with Durable Project State

## Implementation Plan for GitHub Copilot and Visual Studio Code

**Version:** 1.1
**Date:** 2026-10-21
**Initial owner/operator:** Melvin Reyes  
**Purpose:** Build a reusable, project-agnostic framework for planning and implementing small-to-medium MVP projects through supervised specialized agents, durable state, bounded context, deterministic guardrails, progress monitoring, and cost-aware model routing.
**Change:** Added the approved documentation-only T0 verification baseline; no P0 implementation or project-state transition was started.

---

## 1. Executive Summary

The framework replaces a long chain of manually copied prompt sessions with a supervised agent graph whose canonical state history is an append-only transition log in version-controlled project files rather than chat history. `PROJECT_STATE.yaml` is a rebuildable projection maintained by the State Engine.

The architecture uses:

- A low-privilege Project Coordinator
- Specialized agents with explicit capability, identity, and sandbox profiles
- An optional, separately approved read-only subagent pilot
- Agent Skills for reusable project-independent workflows
- Hooks for verified lifecycle-event and tool enforcement, logging, and transition requests
- An append-only transition log, a State Engine-maintained state projection, and task packets
- Human approval gates for consequential actions
- Worktrees and sandboxed execution for implementation tasks
- Approved OpenTelemetry or available provider-/harness-exposed usage signals for token, tool, latency, and model monitoring; estimates and unavailable values are labeled ([ADR 0009](docs/adr/0009-provider-usage-telemetry.md), [ADR 0010](docs/adr/0010-audit-before-usage-telemetry.md))
- Auto Efficiency, Balance, and Intelligence routing policies based on task risk and complexity
- A cross-cutting verification baseline that selects methods by behavior and risk, uses approved seams and independent oracles, separates behavioral evidence from technical enforcement, and gates readiness on credible evidence ([T0 protocol](docs/testing/p0-verification-protocol.md), [ADR 0016](docs/adr/0016-verification-claims-and-acceptance-authority.md))

The framework is not a replacement for GitHub permissions, Azure RBAC, Azure DevOps approvals, branch protection, pipeline environments, or human review.

---

## 2. Design Principles

1. **The append-only transition log is canonical; `PROJECT_STATE.yaml` is its rebuildable projection.**
2. **The Coordinator cannot implement, approve actions, or directly advance project state.**
3. **Every specialist has the smallest practical tool set.**
4. **Instructions guide; hooks enforce only verified coverage, alongside manifests, sandboxes, RBAC, and pipelines.**
5. **Context is assembled per task rather than loading the entire project.**
6. **A subagent report is evidence, never authorization.**
7. **Consequential external actions remain human-confirmed.**
8. **State transitions are deterministic and schema-validated.**
9. **Failures stop safely and produce recovery state.**
10. **Local audit precedes usage telemetry; provider-dependent telemetry is validated before optimization decisions.**
11. **Deterministic scripts perform stable repetitive work; agents perform judgment-heavy work.**
12. **Framework core, project profile, and project extensions have distinct boundaries.** The framework core contains project-independent state, task-context, validation, and enforcement contracts. Project-specific objectives, integrations, risk, context, approvals, targets, and criteria belong in the project profile. Distinct workflows may add agents or skills as project extensions without changing the core. ([ADR 0011](docs/adr/0011-framework-core-and-extensions.md))
13. **Risk profiles can add safeguards but cannot weaken the baseline** for state integrity, explicit scope, audit, or human approval of consequential actions. The project owner classifies the project and confirms additional controls; a lower risk label is not an authorization bypass. ([ADR 0012](docs/adr/0012-risk-profiles-cannot-weaken-baseline.md))
14. **Verification methods fit the behavior and risk.** TDD is used only where deterministic behavior, an approved observable seam, and an independent oracle make it credible; P0 capability checks use documented experiments.
15. **Behavioral evidence is not enforcement evidence.** Instructions guide; a restriction is called enforced only when a supported technical control is verified to block violations. ([ADR 0005](docs/adr/0005-verified-enforcement-boundaries.md), [ADR 0016](docs/adr/0016-verification-claims-and-acceptance-authority.md))
16. **Expected results are independent of the implementation under test.** Tests and reviews do not self-certify acceptance; the State Engine remains the sole writer of canonical state transitions. ([ADR 0002](docs/adr/0002-single-writer-state-transitions.md), [ADR 0016](docs/adr/0016-verification-claims-and-acceptance-authority.md))

---

## 3. Target Architecture

```text
Human Project Owner
        |
        v
Project Coordinator Agent
  |     +------------------> Project State Engine (transition request)
  +------------------------> Specialist recommendation and task packet

Operator selects and starts the recommended specialist session
        |
        v
Specialist Agents
  - Foundation and Governance
  - Research and Discovery
  - Architecture and Planning
  - Controlled Implementation
  - Validation and Evidence
  - Documentation and Handoff

Optional, separately approved post-foundation read-only subagent pilot
  - Repository Explorer
  - Documentation Consistency Reviewer
  - Test/Log Summarizer
  - Security Reviewer

Enforcement Plane
  - Custom-agent tool restrictions
  - Capability, identity, and sandbox profiles
  - Execution manifests and approvals
  - VS Code/Copilot approvals
  - Lifecycle hooks for verified events and tools
  - External RBAC and branch/pipeline protections
  - Append-only audit events

Verification Plane
  - Requirement and risk traceability
  - Approved seams and independent oracles
  - Fit-for-purpose verification methods
  - Separate review axes and evidence correlation
  - Compatibility and regression selection

The Verification Plane grants no authority and cannot advance project state.
```

---

## 4. Reusable Repository Template

```text
.github/
├── agents/
│   ├── project-coordinator.agent.md
│   ├── foundation-governance.agent.md
│   ├── research-discovery.agent.md
│   ├── architecture-planner.agent.md
│   ├── controlled-implementer.agent.md
│   ├── validation-evidence.agent.md
│   ├── independent-review.agent.md
│   └── progress-reporter.agent.md
├── hooks/
├── instructions/
│   ├── security.instructions.md
│   ├── project-state.instructions.md
│   └── context-management.instructions.md
└── skills/
    ├── project-intake/SKILL.md
    ├── state-transition/SKILL.md
    ├── task-packet-builder/SKILL.md
    ├── architecture-decision/SKILL.md
    ├── implementation-plan/SKILL.md
    ├── validation-evidence/SKILL.md
    └── progress-reporting/SKILL.md

project-control/
├── PROJECT_PROFILE.yaml
├── PROJECT_STATE.yaml
├── CURRENT_RUN.yaml
├── AGENT_REGISTRY.yaml
├── CONTEXT_INDEX.yaml
├── APPROVAL_QUEUE.yaml
├── BLOCKERS.yaml
├── COST_BUDGET.yaml
├── DECISION_INDEX.yaml
├── task-packets/
├── runs/
├── progress/
│   ├── dashboard.md
│   ├── dashboard.json
│   └── timeline.jsonl
└── telemetry/
    ├── usage-summary.json
    └── usage-dashboard.md

schemas/
├── project-profile.schema.json
├── project-state.schema.json
├── agent-profile.schema.json
├── run.schema.json
├── task-packet.schema.json
├── approval.schema.json
├── audit-event.schema.json
└── usage-event.schema.json

docs/
├── decisions/
├── handoffs/
├── plans/
├── testing/
│   └── p0-verification-protocol.md
├── compatibility/                 # Sanitized P0 reports as evidence is produced
├── runbooks/
└── retrospectives/

scripts/
├── state/
├── hooks/
├── context/
├── telemetry/
└── validation/

tests/
├── state/
├── hooks/
├── sandbox/
├── agents/
├── fixtures/                      # Add when executable tests require fixtures
└── scenarios/
```

---

## 5. Durable State Model

### 5.1 Project Profile

`PROJECT_PROFILE.yaml` contains project-specific scope without changing the generic framework.

```yaml
project:
  id: example-mvp
  name: Example MVP
  owner: melvin-reyes
  framework_version: "1.0.0"
  classification: internal
  lifecycle: mvp

objectives:
  - deliver-one-validated-use-case

integrations:
  github: true
  azure_devops: false
  azure: false

risk:
  level: medium
  production_actions: prohibited

models:
  default_tier: balance
  high_risk_tier: intelligence
  maintenance_tier: efficiency
```

Each project pins a framework version. Adopting a later version requires a reviewed upgrade or migration with compatibility and security-impact information; project customizations must be preserved, with an explicit path for urgent security fixes. ([ADR 0013](docs/adr/0013-reviewed-framework-upgrades.md))

Risk profiles may add safeguards but cannot remove the framework baseline for state integrity, explicit scope, audit, or human approval of consequential actions. The project owner classifies each project and confirms its additional controls; a lower risk label is not an authorization bypass. ([ADR 0012](docs/adr/0012-risk-profiles-cannot-weaken-baseline.md))

### 5.2 Project State

The append-only transition log is canonical. `PROJECT_STATE.yaml` is a rebuildable state projection maintained by the State Engine.

```yaml
schema_version: 1
project_id: example-mvp
iteration: ITER-001
milestone: M0
phase: M0A
status: ready
state_version: 4
last_event_id: EVT-00021

active_run: null
latest_completed_run: RUN-00003
latest_handoff: docs/handoffs/RUN-00003.md

blockers:
  - BLK-002

pending_approvals:
  - APR-004

security:
  subagents_enabled: false
  sandbox_required_for_terminal: true
  direct_production_actions: prohibited
```

### 5.3 Current Run

`CURRENT_RUN.yaml` contains one bounded assignment, allowed context, tools, outputs, and completion criteria.

### 5.4 Event Sourcing

Every authorized state transition appends an event to `project-control/progress/timeline.jsonl`; the State Engine validates and records transitions and maintains `PROJECT_STATE.yaml` as a rebuildable current view. The event record contains:

- Event and run IDs
- Previous state version
- Requested transition
- Actor/agent
- Approval reference
- Manifest and profile hashes
- Result
- Timestamp
- Previous-event and current-event hash

The State Engine is the sole writer of state events and the rebuildable project-state projection. Agents and the Coordinator submit transition requests with an expected state version; stale requests are rejected for explicit refresh. Agents never edit past events or the projection. ([ADR 0001](docs/adr/0001-event-log-as-state-source.md), [ADR 0002](docs/adr/0002-single-writer-state-transitions.md))

Hash chaining makes changes detectable only relative to a trusted checkpoint; it does not independently prevent complete history replacement. A trusted checkpoint is required to claim tamper evidence; history without one is unverified. Baseline protection against normal writes depends on the State Engine and repository controls. Invalid event history is quarantined; recovery reconstructs or verifies state from valid events and a trusted checkpoint and never replays external actions. Signed or external immutable checkpoints are added only when required by the organizational threat model. ([ADR 0002](docs/adr/0002-single-writer-state-transitions.md), [ADR 0014](docs/adr/0014-tamper-evident-event-history.md))

---

## 6. Agent Catalogue

### 6.1 Project Coordinator

**Purpose:** Determine the next eligible task, assemble context, recommend a specialist and model tier, submit version-bound transition requests, and update non-authoritative progress summaries after validation.

**Allowed:** Read state; create task packets; update non-authoritative draft state; add blockers; produce progress summaries; recommend the next specialist; submit transition requests with the expected state version.

**Denied:** Source implementation, terminal, cloud writes, policy edits, approval creation or approval of actions, deployment, secret access, direct specialist invocation, or directly advancing project state.

For the initial MVP, the Coordinator recommends the next specialist but does not invoke it; specialist invocation remains operator-directed. Any future direct specialist invocation requires a harness prototype demonstrating bounded tools, auditable invocation, and reviewable results. The Coordinator requests a phase transition only when deterministic completion evidence is present; the State Engine alone applies valid transitions, and operator approval is required only at explicit gates. Stale requests require an explicit state refresh. ([ADR 0002](docs/adr/0002-single-writer-state-transitions.md), [ADR 0003](docs/adr/0003-coordinator-boundary.md))

**Default model:** Auto Balance.

**Escalate to Intelligence:** Conflicting state, recovery after partial failure, security-boundary decision, or cross-project architecture.

### 6.2 Foundation and Governance Agent

Creates repository scaffold, schemas, threat model, policies, and tests. No external-system access.

**Default model:** Balance.

### 6.3 Research and Discovery Agent

Inspects repository and approved read-only external metadata. Writes findings/evidence only.

**Default model:** Balance for repository-only discovery; Intelligence for cross-system correlation or ambiguity.

### 6.4 Architecture and Planning Agent

Produces options, ADRs, implementation plan, execution manifest, verification methods, testable acceptance criteria, proposed seams and independent oracles, and rollback. Cannot implement or weaken a control to make it easier to test.

**Default model:** Intelligence.

### 6.5 Controlled Implementation Agent

Makes manifest-approved changes in a sandboxed worktree; runs the approved verification and tests/builds/scans; may prepare PRs. External writes require point-of-action confirmation. It cannot change its task's acceptance criteria or quality gates, weaken tests to obtain green, or self-certify acceptance.

**Default model:** Intelligence.

### 6.6 Validation and Evidence Agent

Read-only validation that correlates requirements, commit, build, artifact, deployment, tests, telemetry, and acceptance evidence; checks evidence sufficiency and reports behavioral evidence separately from enforcement evidence. It cannot modify implementation or accept its own findings.

**Default model:** Intelligence for integrated validation; Balance for deterministic evidence formatting.

### 6.7 Independent Review Agent

Read-only adversarial review of proposed plans or diffs. It reports specification conformance, engineering standards, security boundaries, and test/evidence sufficiency separately, cannot modify authoritative artifacts, and cannot silently redefine requirements.

**Default model:** Intelligence for security/architecture review; Balance for routine diff review.

### 6.8 Progress and Usage Reporter

Generates dashboard and summaries from structured state/events/telemetry. It does not invent status or token values.

**Default model:** Auto Efficiency or Balance.

---

## 7. Capability, Identity, Sandbox, and Delegation Profiles

Every agent definition has a companion machine-readable profile.

```yaml
agent: controlled-implementer
capability:
  allowed_tools: [read, search, approved-edit, approved-terminal]
  allowed_paths: [worktrees/current/**, project-control/runs/current/**]
  prohibited_paths: [.github/hooks/**, project-control/approvals/**]
  confirmation_required: [git-push, create-pr, queue-nonprod-pipeline]
  permanently_prohibited: [production, rbac, secret-read, policy-edit]
identity:
  expected_account: operator-confirmed
  maximum_expected_access: repository-write
  credential_persistence: prohibited
sandbox:
  required: true
  filesystem_default: deny
  network_default: deny
  allowed_domains: []
  host_docker_socket: denied
  elevation: denied
  unavailable_behavior: stop
delegation:
  subagents_allowed: []
  nested_delegation: false
```

Capability profiles declare intended scope; they do not grant or reduce permissions held by the execution identity. A restriction is treated as enforced only when a supported harness, operating-system, or external platform control is verified to block violations. Consequential actions require an effective independent control and are disabled when that control is unavailable. ([ADR 0005](docs/adr/0005-verified-enforcement-boundaries.md))

Before live connections, the organization approves the actual integration identity topology. Use minimum required access, separate read and write identities where the platform supports meaningful separation, and prefer protected pipelines for writes. If an active identity is broader than the approved action and cannot be constrained, that action is unavailable. ([ADR 0006](docs/adr/0006-least-privilege-integration-identities.md))

Verification and review activities use the same capability, identity, and sandbox boundaries as other tasks. A testing or review label grants no tools, paths, network access, identity permissions, approvals, or delegation.

### Delegation policy

For the initial MVP:

- No write-enabled subagents
- No nested delegation
- The optional M1B pilot is not enabled merely because the framework supports subagents; it requires a separate readiness decision and operator approval
- The operator initiates any specialist or subagent session; autonomous routing is deferred
- Subagents cannot update authoritative state
- A subagent is a bounded delegated invocation and does not inherit approval or authority from its parent
- Parent validates subagent output before use
- Each invocation receives a bounded context and emits parent-visible logs

---

## 8. Context Management

### 8.1 Context Index

`CONTEXT_INDEX.yaml` maps each task class to canonical sources, advisory summaries, exclusions, and size budgets.

Context sources declare authority by subject. Summaries are advisory and do not supersede their authoritative sources. ([ADR 0004](docs/adr/0004-version-bound-task-context.md))

```yaml
contexts:
  architecture-planning:
    required:
      - project-control/PROJECT_PROFILE.yaml
      - project-control/progress/timeline.jsonl
      - project-control/PROJECT_STATE.yaml
      - docs/decisions/index.md
    conditional:
      - docs/research/**
    excluded:
      - project-control/runs/**/raw/**
      - logs/raw/**
    max_documents: 10
    max_handoffs: 2
```

### 8.2 Task Packet

Each task packet is immutable and bound to the state version, governing profiles and policies (including their versions or hashes), authoritative context sources, exact execution scope, and completion criteria for which it was prepared. It contains the objective, dependencies, allowed files/tools/targets, exclusions, model tier, outputs, tests, and requested next state. Material implementation tasks also identify the approved verification seam, independent oracle, selected method, acceptance threshold, and required evidence before work begins. A material change to scope, authority, control coverage, observed interface, expected behavior, threshold, or required evidence requires a revised packet; execution-neutral changes may be recorded as an addendum. P0 probes use the P0 protocol, not implementation task packets. Raw logs are excluded by default; included excerpts are bounded, source-linked, and treated as untrusted data. ([ADR 0004](docs/adr/0004-version-bound-task-context.md), [ADR 0016](docs/adr/0016-verification-claims-and-acceptance-authority.md))

### 8.3 Summarization rules

- Raw logs are never default context.
- Decisions are stored as ADRs, not inferred from conversations.
- Every completed run writes a structured result and concise handoff.
- Summaries link source evidence and declare unresolved uncertainty.
- Material context, scope, or control changes invalidate the packet and require a new packet; approval is not transferred to it. ([ADR 0004](docs/adr/0004-version-bound-task-context.md), [ADR 0007](docs/adr/0007-one-time-execution-approval.md))
- Material verification changes follow the packet rules in §8.2. Verification evidence is separate from canonical state events and does not itself authorize a transition.

---

## 9. Hooks and State Transitions

Hooks enforce only lifecycle events and tools demonstrated to pass through the selected harness. Operations outside verified hook coverage remain governed by tool removal, sandboxing, external authorization, protected workflows, and manual approval.

The State Engine is the sole writer of state events and the rebuildable projection; agents and the Coordinator submit transition requests with an expected state version, and stale requests are rejected for explicit refresh. ([ADR 0002](docs/adr/0002-single-writer-state-transitions.md))

Run statuses have the following meanings: `blocked` means a prerequisite or control prevents progress; `failed` means the task did not complete and effects are known; `needs-review` means effects or state history are uncertain; and `rolled-back` means rollback was performed and its outcome verified. Resumption after review requires operator authorization. Changed scope or invalid history requires a new run, and recovery never automatically retries external actions. ([ADR 0015](docs/adr/0015-run-status-and-recovery-semantics.md))

Test statuses are defined separately in the [P0 verification protocol](docs/testing/p0-verification-protocol.md); they are not run statuses or state-transition events. Evidence records correlate the applicable requirement/control, check, run and packet references, environment versions, result, and evidence location. Hooks may record test activity only for verified events/tools that pass through them; an absent hook event does not prove no activity occurred. ([ADR 0002](docs/adr/0002-single-writer-state-transitions.md), [ADR 0005](docs/adr/0005-verified-enforcement-boundaries.md))

### `sessionStart`

Verify repository, branch/worktree, state version, immutable task packet, agent/profile hashes, identity metadata, sandbox readiness, and telemetry correlation. Request that the State Engine record the run-start transition through a deterministic state command.

### `userPromptSubmitted`

Hash/sanitize prompt, bind to run, detect scope expansion, and reject requests that conflict with the task packet.

### `preToolUse`

Validate exact tool, arguments, paths, targets, domains, profile, manifest, and approval. Treat a restriction as enforced only where the relevant control is verified to block violations; require an effective independent control for consequential actions and disable them if unavailable. Return allow, deny, or confirmation-required. ([ADR 0005](docs/adr/0005-verified-enforcement-boundaries.md))

Each approval authorizes one exact execution scope and is bound to the task packet and manifest, expected state version, operator identity, repository/branch, paths, tools/actions, targets/domains, and expiry. Scope changes require a new approval. Expired, revoked, consumed, or mismatched approvals are rejected; handoffs and subagent results never transfer approval. ([ADR 0007](docs/adr/0007-one-time-execution-approval.md))

### `postToolUse`

Record sanitized result, duration, changed files, remote effects, tests, and evidence. Detect unexpected scope.

### `errorOccurred`

Classify the run as blocked when a prerequisite/control prevents progress, failed when the task did not complete and effects are known, needs-review when effects or state history are uncertain, or rolled-back only when rollback was performed and verified. Preserve partial-effect evidence, quarantine invalid event history, and do not automatically retry or replay external actions. Resumption after review requires operator authorization; changed scope or invalid history requires a new run. ([ADR 0002](docs/adr/0002-single-writer-state-transitions.md), [ADR 0015](docs/adr/0015-run-status-and-recovery-semantics.md))

### `sessionEnd`

Validate completion criteria, request the State Engine to record a state event, generate handoff, refresh dashboard, summarize telemetry, and recommend the next eligible run without auto-approving it.

Hooks are harness-specific. Implement one adapter per supported harness and certify each adapter with contract tests; do not treat unverified lifecycle events or tools as hook-enforced.

---

## 10. Sandboxing Strategy

### MVP baseline

- Triage/Planning: eliminate terminal where possible
- Research: sandbox parsers; bounded read endpoints
- Implementation: required sandbox and disposable worktree
- Validation: sandbox evidence normalization
- MCP: review separately from terminal sandbox

MCP servers and tools are independent execution and access boundaries, not contained by the terminal sandbox. Keep them disabled until server/tool source and version, identity, credentials, network, logging, read/write separation, and relevant controls have been reviewed and verified. ([ADR 0008](docs/adr/0008-independent-mcp-boundary.md))

### Required tests

Select sandbox checks by control claim and risk. For a critical enforcement claim, use applicable allowed-action, denied-action, bypass, fail-closed, evidence, and partial-effect recovery checks. The cases below are candidates, not claims that the selected harness supports or enforces them. Do not claim an untested boundary is enforced.

- Denied home, SSH, cloud CLI, GitHub CLI, kubeconfig, and other-repository paths
- Approved path reads/writes
- Network deny and explicit allowlist
- Child-process inheritance
- Symlink/path traversal
- Shell chaining and command substitution
- Sandbox unavailable or bypass requested
- No unsandboxed fallback
- Docker daemon/socket denied unless separately approved

Prefer protected pipelines for resource-intensive builds and deployments.

---

## 11. Framework Verification and Testing Strategy

The [P0 verification protocol](docs/testing/p0-verification-protocol.md) is normative for P0 evidence and provides lightweight guidance for later implementation work. Select verification per behavior, risk, approved seam, independent oracle, and available evidence; TDD is selective and is not a P0 requirement. Critical enforcement claims require applicable allow, deny, bypass, fail-closed, evidence, and recovery checks. Test statuses are distinct from run statuses, and only `PASS` satisfies a required check.

Material reviews report specification conformance, engineering standards, security boundaries, and test/evidence sufficiency as separate axes. Implementation agents cannot self-certify. For this single-owner MVP, the project owner/operator performs a separate evidence-review step and explicitly accepts P0H; this is procedural separation, not independent human or identity-level separation. A waiver cannot clear a critical P0 gap or permit specification. Only a verified external control, accepted and carried into the specification, may support `READY_FOR_SPEC_WITH_EXTERNAL_CONTROLS`. See [ADR 0016](docs/adr/0016-verification-claims-and-acceptance-authority.md).

This section does not assert any unverified product capability. Detailed implementation testing procedures, machine-readable gates, schemas, broad mutation testing, and agent-evaluation infrastructure remain deferred.

## 12. Progress Monitoring

### Dashboard

Generate `project-control/progress/dashboard.md` and JSON from state/events.

Required sections:

- Current milestone/phase/run/agent
- Overall and per-phase progress
- Completed deliverables
- Active blockers and approvals
- Security gates
- Latest tests
- Usage/budget status
- Next recommended action

### Progress calculation

Do not ask the model to estimate percentages. Each phase defines weighted deterministic completion criteria. The reporting script calculates progress from passed criteria.

### Agents Window and session monitoring

Use the VS Code Agents/session surfaces where available for active sessions. The repository dashboard is an advisory summary derived from the canonical transition log and other authoritative sources, not an authority of its own.

---

## 13. Token, Cost, and Performance Monitoring

### Telemetry

Before specialist work, require the applicable local audit and policy/sandbox denial tests; optional provider token, model, or cost telemetry is not a prerequisite for read-only fixture work. Keep local audit events distinct from provider-usage telemetry. Defer provider-usage telemetry proof to M3A. ([ADR 0010](docs/adr/0010-audit-before-usage-telemetry.md))

Telemetry reports only provider- or harness-exposed facts and labels estimates and unavailable values explicitly. Provider billing is authoritative for premium requests and cost. Prompt and tool-output capture is off by default. Telemetry export requires organizational approval of fields, storage, access, and retention. Capture approved run/session/agent/model/tool correlations, exposed token counts, latency, cache metrics, failures, denials, and subagent spans only within those limits. ([ADR 0009](docs/adr/0009-provider-usage-telemetry.md), [ADR 0010](docs/adr/0010-audit-before-usage-telemetry.md))

### Usage files

- `usage-summary.json`: machine-generated totals
- `usage-dashboard.md`: human-readable view
- `COST_BUDGET.yaml`: budgets and alerts

```yaml
iteration: ITER-001
budgets:
  input_tokens: 300000
  output_tokens: 80000
  intelligence_runs: 12
alerts:
  warning_percent: 75
  stop_percent: 100
```

Treat premium requests and billing as provider-reported values. Do not infer exact cost from estimated text length. Redact prompts/tool outputs from telemetry unless capture is explicitly approved.

---

## 14. Auto Model Tier Policy

### Efficiency

Use for formatting, deterministic dashboard regeneration, file moves, repetitive checklist updates, short summaries, and easy-to-verify boilerplate.

### Balance

Default for Coordinator tasks, repository inventory, scaffolding, routine documentation, fixtures, ordinary tests, and bounded debugging.

### Intelligence

Use for threat modeling, architecture, security controls, ambiguous discovery, manifest/policy logic, implementation, incident recovery, complex debugging, and evidence correlation.

### Escalation rule

Agents cannot change tier merely to bypass a refusal or failed guardrail. Escalation is recorded in the run and limited to documented conditions. Auto tiers express routing preference, not a guaranteed model or spending ceiling.

---

## 15. Implementation Milestones and Copilot Sessions

### Pre-Specification Product-Enforcement Gate

Before the framework specification and M0A implementation begin, complete the
P0 product-enforcement milestone against the selected Visual Studio Code and
GitHub Copilot environment.

P0 verifies product capabilities and produces the compatibility baseline. It
does not implement framework components.

- P0A: Environment and harness inventory
- T0: Test Strategy, Feedback Loops, and Verification Baseline. This is an approved documentation-only readiness workstream after P0A and before substantive P0B verification, not a project-state milestone. T0 does not advance canonical state and remains incomplete until the project owner accepts the documentation handoff.
- P0B: Tool and hook coverage
- P0C: Sandbox enforcement
- P0D: Approval binding and replay rejection
- P0E: Local audit and trusted checkpoint feasibility
- P0F: Telemetry and context-sizing feasibility
- P0G: MCP boundary verification, only if MCP is required
- P0H: Compatibility synthesis and specification-readiness decision

M0C-A and M0C-B occur after specification. They implement the controls whose
feasibility was established during P0.

P0H returns one of `READY_FOR_SPEC`, `READY_FOR_SPEC_WITH_EXTERNAL_CONTROLS`,
`NOT_READY_CRITICAL_CONTROL_GAP`, or `NOT_READY_INCOMPLETE_EVIDENCE`. Only the
first two permit formal framework specification/readiness. Any external or
compensating control must be explicit, verified, evidenced, accepted by the
project owner, and carried forward as a binding specification requirement. A
waiver alone cannot clear a critical gap. Unresolved mandatory review findings
map to the applicable not-ready outcome. P0A evidence remains unchanged. P0B
was started and is closed by the owner as bounded partial verification;
P0C–P0H remain not started, and P0H acceptance has not occurred.

The owner has decided that the minimum MVP may proceed without hook enforcement
as a security boundary. This does not create a P0H outcome or clear the formal
specification/readiness gate. Separately approved, bounded minimum-MVP planning
or implementation tasks may proceed before P0H only when they do not materially
depend on unverified hook behavior, use task packets with explicit scope and
acceptance evidence, and retain owner review, bounded sessions, synthetic data,
isolation where appropriate, no production or external writes, diff review,
explicit approval for consequential actions, and State Engine validation for
canonical state when implemented. No unverified behavior may be represented as
`ENFORCED`. Capabilities depending on hook-based restriction, denial,
fail-closed behavior, lifecycle correlation, or unverified target coverage
remain disabled, owner-gated, or separately validated before use.

This allowance is not a blanket start authorization. A specific M0 task must
identify whether it depends on any unverified control and remain within the
approved minimum-MVP boundary. M0C-A enforcement, audit, and approval-dependent
operational specialist gates remain in force; no operational specialist may
begin before the applicable local audit, approval, and verified policy/sandbox
denial/failure controls pass.

### M0A: Repository and State Foundation

**Copilot tier:** Balance  
**Deliverables:** scaffold, schemas, project profile/state, event format, validators, tests.

### M0B: Agent Registry and Context Compiler

**Tier:** Balance  
**Deliverables:** agent registry, context index, task-packet builder, context-budget checks, handoff/result schemas.

### M0C-A: Enforcement, Local Audit, and Approval Prototype

**Tier:** Intelligence  
**Deliverables:** policy engine, verified-coverage harness adapter, local append-only audit events and local audit-event schema, approval prototype, and lifecycle/tool enforcement tests. ([ADR 0005](docs/adr/0005-verified-enforcement-boundaries.md), [ADR 0010](docs/adr/0010-audit-before-usage-telemetry.md))

**Gate:** No operational specialist before local audit, approval, and verified policy/sandbox enforcement denial/failure tests pass. This gate is not waived by the bounded minimum-MVP allowance. Local audit is required before specialist work; optional provider token, model, or cost telemetry is not a prerequisite for read-only fixture work. This milestone does not prove provider token or model telemetry. ([ADR 0005](docs/adr/0005-verified-enforcement-boundaries.md), [ADR 0010](docs/adr/0010-audit-before-usage-telemetry.md))

### M0C-B: Sandbox, Identity, and MCP-Boundary Prototype

**Tier:** Intelligence  
**Deliverables:** sandbox profiles/tests, execution identity validation and organization-approved identity topology, MCP-boundary review and tests, and failure recovery. MCP remains disabled until server/tool source and version, identity, credentials, network, logging, read/write separation, and relevant controls are reviewed and verified. ([ADR 0006](docs/adr/0006-least-privilege-integration-identities.md), [ADR 0008](docs/adr/0008-independent-mcp-boundary.md))

**Gate:** No operational specialist before the applicable sandbox, identity, and MCP-boundary tests pass.

### M1A: Coordinator and Read-Only Specialists

**Tier:** Intelligence  
**Deliverables:** Coordinator, Foundation, Research/Discovery, Planning agents, and fixture workflow. Coordinator recommends specialist; operator selects and starts specialist; autonomous routing is deferred; no delegated authorization exists.

### M1B: Optional Read-Only Subagent Pilot

**Tier:** Intelligence  
**Deliverables:** only if separately approved, one repository-explorer or reviewer subagent with no nested delegation, bounded context, connected logs, explicit operator-started invocation, no inherited approval or authority, and a benchmark against the single-agent path.

**Gate:** Remains disabled unless the operator separately approves the pilot; keep only if quality/time/context benefit is measurable.

### M2A: Controlled Implementation Fixture

**Tier:** Intelligence  
**Deliverables:** worktree workflow, manifest enforcement, sandboxed edits/tests, mocks, negative tests, no live repository writes.

### M2B: Narrow Live Nonproduction Pilot

**Tier:** Intelligence  
**Deliverables:** organization-approved least-privilege identity topology, approved repository/branch, PR preparation, optional confirmed push/PR, and external writes through protected workflows. An action is unavailable if the active identity is broader than its approved scope and cannot be constrained. ([ADR 0006](docs/adr/0006-least-privilege-integration-identities.md))

### M3A: Validation, Evidence, and Progress

**Tier:** Intelligence for logic; Balance/Efficiency for reporting  
**Deliverables:** acceptance evidence, tamper-evident correlations verified relative to trusted checkpoints, closure state, deterministic dashboard, usage report, and provider-/harness-dependent token and model telemetry proof. Without a trusted checkpoint, history integrity remains unverified. ([ADR 0009](docs/adr/0009-provider-usage-telemetry.md), [ADR 0014](docs/adr/0014-tamper-evident-event-history.md))

### M3B: Generic Project Template

**Tier:** Balance  
**Deliverables:** template repository, new-project bootstrap skill, customization guide, sample low/medium/high-risk profiles.

### M4: Hardening and Limited Team Readiness

**Tier:** Intelligence for risk review; Balance for docs  
**Deliverables:** retrospective, least-privilege reductions, support/incident runbook, team go/no-go package.

---

## 16. Session Strategy

Use one fresh Copilot session per milestone or bounded sub-milestone, not per small task. The Coordinator-generated task packet carries context. If a session exceeds budget or reaches a security boundary, stop, write a result/handoff, and create a continuation run rather than relying on chat history.

Complete and accept the T0 documentation baseline before starting substantive P0B verification. P0A evidence is preserved unchanged. A separate project-owner/operator evidence-review step is required for P0H; this is procedural/session-level separation only.

The sequence below describes the formal framework build. The owner’s bounded
P0B closure does not require completing B3–B7 before beginning the minimum MVP.
Before formal P0H acceptance, the owner may separately approve a minimum-MVP
planning or implementation task only when its task packet shows that it does
not materially depend on unverified hook behavior and preserves the controls
and exclusions in §15. This narrow lane does not start the full framework
specification, accept readiness, or permit M0C-A/M0C-B enforcement-dependent
or operational-specialist work. The owner’s immediate next step is to define
and approve the exact bounded minimum-MVP task packet; implementation is not
authorized by this plan paragraph alone.

Formal framework build sequence:

1. P0A-P0H: product-enforcement verification and compatibility baseline
2. Framework specification after P0H returns either approved ready outcome and the specification is approved
3. Approved implementation tickets
4. M0A
5. M0B
6. M0C-A: enforcement, local audit, and approval prototype
7. M0C-B: sandbox, identity, and MCP-boundary prototype
8. M1A
9. M1B optional
10. M2A
11. M2B
12. M3A: validation, evidence, progress, and usage reporting
13. M3B
14. M4

---

## 17. Generic Bootstrap Workflow for Future MVPs

1. Create repository from template.
2. Complete `PROJECT_PROFILE.yaml`.
3. Run Project Intake Skill.
4. Coordinator validates profile and proposes milestone graph.
5. Owner approves project graph and risk posture.
6. Coordinator creates first task packet.
7. Operator selects and starts the recommended specialist session; specialist executes the bounded task.
8. Hooks log and enforce only lifecycle events and tools verified to pass through the selected harness; other operations rely on tool removal, sandboxing, external authorization, protected workflows, and manual approval.
9. Validator checks completion criteria.
10. The State Engine validates and records the requested transition or rejects it.
11. Dashboard and usage report refresh.
12. Repeat until MVP exit criteria are met.

---

## 18. MVP Exit Criteria for the Framework

- A new sample project can be bootstrapped without editing framework internals.
- Fresh sessions recover correctly from canonical event history and its rebuildable projection.
- Coordinator cannot implement, approve actions, or directly advance project state.
- Restrictions on tools, paths, and targets are treated as enforced only where supported harness, OS, or external controls are verified to block violations; consequential actions require an effective independent control and are disabled if it is unavailable. ([ADR 0005](docs/adr/0005-verified-enforcement-boundaries.md))
- State transitions are schema-valid, auditable, and recoverable.
- Sandbox failures stop actions that depend on the sandbox; hooks stop only covered lifecycle events and tools, while uncovered operations rely on tool removal, sandboxing, external authorization, protected workflows, and manual approval.
- The optional read-only subagent pilot remains disabled unless separately approved; if enabled, the subagent cannot write or delegate and inherits no approval or authority.
- Progress derives from deterministic criteria.
- Token/tool/model data is observable where provider or harness telemetry exposes it; estimates and unavailable values are labeled. ([ADR 0009](docs/adr/0009-provider-usage-telemetry.md))
- Critical requirements and controls have credible, traceable verification evidence; behavioral and enforcement claims are reported separately, and no control is labeled enforced without verified technical evidence.
- Required acceptance evidence is reviewed on the four separate axes; no unresolved critical finding remains, and any accepted external control is explicit in the specification.
- One controlled nonproduction change completes with approval, rollback, and evidence.
- Documentation is sufficient to repeat the workflow for another MVP.

---

## 19. Key Risks

- **Coordinator becomes overprivileged:** keep coordination tools separate from implementation.
- **State files become inconsistent:** schema validation, state versioning, event sourcing, and the State Engine as sole writer of events and projection; stale transition requests are rejected for explicit refresh. ([ADR 0001](docs/adr/0001-event-log-as-state-source.md), [ADR 0002](docs/adr/0002-single-writer-state-transitions.md))
- **Prompt injection:** treat repository/external content as data; instructions and profiles do not grant authority, and hooks enforce only verified lifecycle-event and tool coverage.
- **Context overgrowth:** context index, task packets, budgets, summaries, and raw-log exclusion.
- **Sandbox overconfidence:** maintain external RBAC and pipeline protections.
- **Subagent opacity:** keep the optional pilot disabled unless separately approved; if enabled, require connected logs, no nested delegation, bounded context, and no inherited approval or authority.
- **Telemetry leaks:** redact, minimize, secure exporter, and require approval.
- **Framework overengineering:** implement milestones incrementally and benchmark value.
- **Framework drift:** pin each project to a framework version and adopt later versions only through a reviewed upgrade/migration that includes compatibility and security-impact information, preserves customizations, and provides an explicit urgent-security-fix path. ([ADR 0013](docs/adr/0013-reviewed-framework-upgrades.md))
- **Product evolution:** pin/certify supported harness versions and rerun compatibility tests after upgrades.
- **Tautological or implementation-coupled tests:** require independent oracles and prefer approved observable seams.
- **False red phase or test weakening:** validate the intended red failure and review test changes; do not weaken tests to obtain green.
- **Self-certification or evidence inflation:** keep implementation separate from acceptance and prioritize credible critical-control evidence over test count.
- **Behavior/enforcement confusion:** report agent behavior separately from verified technical or external controls.

---

## 20. Framework Build Prompt

### Recommended First Framework Build Prompt

Use this prompt only after:

1. P0H returns `READY_FOR_SPEC`.
2. The implementation plan is updated from the compatibility findings.
3. The specification is approved.
4. The implementation tickets are approved.
5. M0A is selected as the first implementation ticket or milestone.

```text
Implement milestone M0A of the Generic Supervised Agent Graph framework.

Read this implementation plan. Do not start M0B or later work.

First inspect the repository and propose the exact M0A file plan. Wait for my approval before editing.

M0A must create:
- the generic repository scaffold;
- PROJECT_PROFILE.yaml and PROJECT_STATE.yaml examples;
- project-profile, project-state, run, event, and task-packet schemas;
- deterministic validators and state-transition fixtures;
- an append-only timeline fixture;
- foundational security and context instructions;
- tests for valid transitions, stale state versions, malformed state, unauthorized transitions, and tampering detection relative to a trusted checkpoint (not prevention of complete history replacement);
- docs/handoffs/M0A-handoff.md.

Use Auto Balance. Escalate to Auto Intelligence only if a security or state-machine ambiguity is discovered, and record the reason before escalating.

Do not create operational agents, hooks that affect live sessions, MCP connections, cloud access, repository writes outside this project, subagents, sandbox exceptions, or telemetry exporters.

All examples must use placeholders and contain no credentials or real tenant/resource information.

Completion requires passing local validation, re-reading generated files, and a handoff containing files changed, tests, decisions, blockers, and the exact recommended M0B entry condition.
```

---

## 21. Current Product References to Validate During Implementation

- VS Code custom agents and handoffs
- GitHub custom-agent configuration and explicit tools
- VS Code/GitHub hook implementation for the selected harness
- GitHub Copilot SDK custom agents/subagents if programmatic orchestration becomes necessary
- Agent Skills for reusable workflows
- VS Code agent sandboxing and approvals
- Copilot Auto model-selection tiers
- GitHub/VS Code telemetry and organizational policy

Revalidate documentation before each security-sensitive milestone because preview and harness-dependent behavior can change.

Compatibility retesting is impact-based: rerun affected P0 checks when relevant selected harness/model, hook, sandbox, operating-system/runtime, approval, identity, MCP, policy, or configuration versions change. Record the impact assessment and exclusions; broad, unknown, or cross-cutting impact requires full P0 recertification.
