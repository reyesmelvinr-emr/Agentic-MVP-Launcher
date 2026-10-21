# Security Policy

## Development Boundary

Agentic MVP Launcher is currently under architecture and product-capability
verification.

The following actions are prohibited during P0:

- Production access or deployment
- Azure, Azure DevOps, GitHub, or Wiz state changes
- RBAC or permission changes
- Secret or credential retrieval
- Real pipeline execution
- Real service-connection use
- External MCP server use unless separately approved
- Host Docker socket mounting
- Unsandboxed fallback for a task requiring sandbox enforcement

## Credential Handling

Never commit:

- Tokens
- Passwords
- Private keys
- Certificates containing private material
- Connection strings
- Cloud CLI caches
- GitHub CLI credentials
- Kubernetes configurations
- Raw environment-variable dumps

## Evidence

Only sanitized prototype or compatibility evidence may be committed. P0 behavioral evidence and technical enforcement evidence must be reported separately. An agent refusal, compliant response, instruction, profile, or unverified hook event does not prove that a prohibited action is technically blocked.

Sanitized P0 compatibility reports belong under `docs/compatibility/` as results are produced; do not fabricate reports or verification results. Raw evidence, raw telemetry, and sensitive logs must be stored in an approved restricted location outside Git and excluded from source control. Reference restricted evidence only by a non-sensitive evidence ID. Never store passwords, tokens, credentials, authentication caches, or unnecessary sensitive raw evidence in Git.

The current phase status is P0A complete, the T0 documentation baseline accepted, P0B–P0H not started, and framework specification and implementation not started. P0B remains blocked until the authoritative P0A evidence location or non-sensitive reference is confirmed. T0 does not advance canonical project state; the State Engine remains the sole writer of state transitions and the rebuildable projection. See the [T0 acceptance record](docs/reviews/T0_DOCUMENTATION_BASELINE_ACCEPTANCE.md), [P0 verification protocol](docs/testing/p0-verification-protocol.md), and [ADR 0016](docs/adr/0016-verification-claims-and-acceptance-authority.md).

## Security-Control Changes

Changes to hooks, capability profiles, identity profiles, sandbox profiles, approval logic, state-transition rules, and audit controls require a dedicated review.

## Accidental Exposure

If a credential or sensitive value is exposed:

1. Stop the current agent session.
2. Do not commit the value.
3. Revoke or rotate the affected credential.
4. Remove the value from local files and logs.
5. Record the incident without repeating the secret.
6. Review whether additional redaction controls are required.
