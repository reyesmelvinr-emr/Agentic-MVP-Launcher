# Wiz Vulnerability Remediation Agents
## Non-Copilot Tasks and Prerequisite Checklist

**Version:** MVP 1.2  
**Owner:** Melvin Reyes

## Project-Wide Responsibilities

- [ ] Review each plan, manifest, capability profile, identity profile, and sandbox profile.
- [ ] Confirm sensitive actions at execution time.
- [ ] Verify actual signed-in identities and external permissions.
- [ ] Verify no secrets enter prompts, files, screenshots, or logs.
- [ ] Keep production and Wiz mutation manual.
- [ ] Review handoffs, diffs, denials, and logs.
- [ ] Treat RBAC/pipeline controls as authoritative.
- [ ] Treat worktrees as source-state isolation only.
- [ ] Deny persistent credentials and host Docker socket by default.
- [ ] Review MCP isolation separately.

## Before S00

- [ ] Create/clone approved private repository.
- [ ] Confirm data classification and evidence-storage rules.
- [ ] Install approved VS Code/Copilot and record versions.
- [ ] Confirm custom agents and selected harness.
- [ ] Record OS and sandbox prerequisites.
- [ ] Determine whether preview sandbox features are permitted.
- [ ] Determine whether Dev Containers and ephemeral containers are permitted.
- [ ] Configure branch/ruleset, force-push prevention, secret scanning, and private visibility.
- [ ] Decide raw/sanitized log locations and retention.

## Before/After S01

- [ ] Approve prohibited, auto-allowed, and confirmation-required actions.
- [ ] Approve manifest lifetime and fail-closed behavior.
- [ ] Approve capability, identity, and sandbox templates.
- [ ] Identify denied credential and unrelated-workspace paths.
- [ ] Configure network policy separately; verify its effective state.
- [ ] Decide MCP server sandbox/trust approach.
- [ ] Test allowed reads and denied tools/actions.
- [ ] Test expired/modified manifests and governance protection.
- [ ] Test denied paths, credential stores, domains, local network, and child processes.
- [ ] Verify sandbox/hook/log failure blocks risky execution.
- [ ] Verify no unsandboxed fallback and no subagent delegation.
- [ ] Inspect logs for secrets and commit accepted baseline.

## Before S02

- [ ] Prepare sanitized UAT finding.
- [ ] Identify Azure tenant/subscription/resource, GitHub organization/repositories, and Azure DevOps organization/project.
- [ ] Approve read-only mechanisms and identities.
- [ ] Install/sign in to approved Azure/GitHub/Azure DevOps tools interactively; do not store tokens in repository.
- [ ] Approve Discovery endpoint allowlist and case-output paths.
- [ ] Confirm image pull is disabled unless separately planned.
- [ ] Review each live command and verify exact resource ID.
- [ ] Compare generated mapping manually with portals and repositories.

## Before S03

- [ ] Resolve ambiguous source/artifact/pipeline ownership.
- [ ] Capture current immutable artifact and rollback artifact.
- [ ] Gather application tests, maintenance windows, and support needs.
- [ ] Select remediation and approve exact tools, commands, paths, targets, domains, credentials, Docker policy, sandbox behavior, tests, rollback, and confirmations.
- [ ] Review manifest outside chat, verify hash, and record approval.

## Before S04

- [ ] Confirm approved manifest/approval remain valid and unmodified.
- [ ] Confirm rollback artifact is available.
- [ ] Verify clean target repository and protected branch policy.
- [ ] Create/verify disposable worktree.
- [ ] Verify sandbox with fixtures before live code.
- [ ] Verify Git/Docker/scanner/build readiness without exposing credentials.
- [ ] Confirm host Docker socket and persistent credentials remain denied unless separately approved.
- [ ] Manually confirm branch creation, push, PR, image push, pipeline queue, UAT request, config/restart request, and rollback request.
- [ ] Review all diffs, scanner evidence, artifact identity, and logs before merge.

## Before S05

- [ ] Confirm merged change, build ID, immutable artifact, UAT pipeline/stage/resource, and rollback.
- [ ] Establish telemetry baseline and observation window.
- [ ] Approve bounded telemetry queries, output limits, evidence endpoints/paths, and sandboxed processors.
- [ ] Manually approve UAT deployment/stage during initial iterations.
- [ ] Validate startup, health, triggers, dependencies, identity, telemetry, and smoke tests.
- [ ] Verify fixed package and deployed artifact match.
- [ ] Review Wiz after scan interval or trigger approved rescan manually.
- [ ] Keep status pending while Wiz detects vulnerable state.
- [ ] Store raw evidence in restricted location and commit sanitized summary.

## Before S06

- [ ] Complete multiple categories where possible.
- [ ] Gather after-action reviews, false blocks, unsafe gaps, sandbox denials, metrics, costs, and maintenance effort.
- [ ] Decide which manual controls remain valuable.
- [ ] Review any proposed reduction in guardrails separately.
- [ ] Evaluate Dev Container/ephemeral-container value from evidence.

## Before S07

- [ ] Confirm several stable pilots and no high-risk gaps.
- [ ] Identify sponsor, policy owner, maintainers, operators, approvers, security reviewer, and auditor.
- [ ] Define separate identities and least-privilege roles.
- [ ] Approve centralized logs, retention, supported hosts/harnesses, sandbox policy, MCP policy, approved images, incident response, and emergency disablement.
- [ ] Obtain GitHub/Azure/Azure DevOps/Wiz/security/privacy approvals before onboarding users.

## Session Readiness Template

```markdown
# Session Readiness: <ID>
- Date/operator/mode:
- Repository/branch/worktree:
- Harness/VS Code/Copilot/OS versions:
- Previous handoff reviewed:
- Capability profile approved:
- Identity and actual signed-in account verified:
- Sandbox effective policy verified:
- Approved paths/domains/tools/actions:
- Explicit prohibitions:
- Known blockers:
- Decision: Ready / Not Ready
```

## Manual Stop Conditions

Unexpected identity/target; secret output; broader tool/path/domain; invalid manifest; sandbox/hook/logger failure; bypass request; partial remote write; unrelated files; governance modification; artifact mismatch; health regression; or Wiz still vulnerable.

## References

- https://code.visualstudio.com/docs/agents/run/agent-sandboxing
- https://code.visualstudio.com/docs/agents/run/security
- https://code.visualstudio.com/docs/agents/run/approvals
