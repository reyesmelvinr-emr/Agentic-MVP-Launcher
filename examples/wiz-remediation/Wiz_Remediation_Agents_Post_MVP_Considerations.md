# Wiz Vulnerability Remediation Agents
## Post-MVP Considerations and Evolution Roadmap

**Version:** 1.0  
**Owner:** Melvin Reyes

## 1. Entry Criteria

Do not expand autonomy until multiple finding categories complete end to end; profiles match actual behavior; negative tests pass; sandbox denials and false blocks are understood; no high-risk identity/secret/target/audit gaps remain; immutable artifact correlation and nonproduction rollback work; closure evidence is complete; and a formal go/no-go approves expansion.

## 2. Sandboxed Subagents

Potential read-only uses: independent GitHub/Azure/Azure DevOps discovery, alternative research, independent diff review, parallel evidence collection, and adversarial fixture testing.

Each subagent needs a separate explicit tool/capability/sandbox profile, no nested delegation, read-only identity, narrow I/O contract, case/correlation ID, parent-visible logs, output integrity, cost/context budget, and no approval/implementation/deployment/rollback/closure authority. Parent agents treat results as untrusted evidence, not authorization.

## 3. Dev Container Standardization

Consider pinned approved tool versions, minimal mounts, no persistent cloud credentials, approved package sources, inventory/SBOM, and reproducible tests. Deny host Docker socket by default. Prefer protected remote builders/pipelines when container builds would weaken local isolation.

## 4. Ephemeral and Cloud Sandboxes

Review preview status, enterprise policy, billing, data residency/retention, repository transfer, firewall, credential brokering, logs, cleanup, scanners/builders, incident response, and forensic access. Prefer existing protected CI/CD over agent-created VMs.

## 5. MCP Hardening

Verify source/version, tool schemas, credentials, network, read/write separation, logs/redaction, Workspace Trust, prompt-injection handling, and confused-deputy scenarios. Terminal sandboxing is not assumed to contain MCP servers.

## 6. Identity Maturity

Introduce separate read-only discovery/validation identity, repository-write identity, nonproduction pipeline service identity, production approval/deployment identity, and separate Wiz read/closure roles. Prefer PR preparation plus constrained pipeline deployment over direct Azure mutation.

## 7. Team Operating Model

Assign policy owner, maintainer, operators, approvers, security reviewer, and auditor. Add peer review, central restricted evidence, retention/deletion/access reviews, supported versions, onboarding, incident/disablement/rollback runbooks, governance ownership, and kill switch outside agent control.

## 8. Automation Expansion Order

1. Read-only discovery
2. Evidence normalization
3. Sandboxed independent review
4. PR preparation
5. Confirmed push/PR
6. Confirmed nonproduction pipeline queue
7. Automated post-deployment evidence

Production, RBAC, secrets, network changes, exceptions, and Wiz closure remain manual unless separately governed; some may remain permanently manual.

## 9. Supply-Chain and Policy Hardening

Consider signed policies/manifests, reproducible tool images, SBOM/provenance, dependency pinning, signed commits/tags, central policy distribution, tamper-evident logs, artifact attestations, immutable deployment references, and drift detection.

## 10. Metrics

Track discovery accuracy, source mapping, cycle time, tool/sandbox denials, false blocks/gaps, approvals, redaction events, replans, rollback readiness, reopened findings, model/cloud/build/storage cost, subagent overhead, and operator time saved versus maintenance.

## 11. Incident Readiness

Plan and exercise partial writes, failed deployments, wrong target, secret exposure, sandbox/hook bypass, audit failure/tampering, prompt injection, agent regression, MCP compromise, emergency disablement, and credential revocation.

## 12. Decision Gate for Every Expansion

Document measurable benefit, evidence, tools/identities/paths/domains/data, threat-model changes, sandbox/authorization changes, tests/rollback, cost/owner, pilot scope/expiration, approver, success criteria, and removal criteria. If it cannot be constrained, observed, tested, and rolled back, do not enable it.

## 13. Recommended First Experiments

1. Read-only reviewer subagent against fixture diffs.
2. Ephemeral container for policy tests/evidence normalization.
3. Dev Container without cloud credentials or host Docker socket.
4. Protected pipeline for one build/scan workload.
5. Separate read-only discovery identity.
6. Central sanitized summaries plus restricted raw evidence.
7. Adversarial prompt-injection and sandbox-boundary assessment.

## 14. References

- https://code.visualstudio.com/docs/agents/run/agent-sandboxing
- https://code.visualstudio.com/docs/agents/run/subagents
- https://code.visualstudio.com/docs/agents/run/security
- https://docs.github.com/en/copilot/concepts/security-governance-and-network-settings/about-cloud-and-local-sandboxes
- https://docs.github.com/copilot/customizing-copilot/customizing-or-disabling-the-firewall-for-copilot-coding-agent
