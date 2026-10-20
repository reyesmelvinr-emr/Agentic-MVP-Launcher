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

Only sanitized prototype evidence may be committed.

Raw evidence, raw telemetry, and sensitive logs must be stored in an approved restricted location and excluded from source control.

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
