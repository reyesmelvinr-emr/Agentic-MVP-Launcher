# P0 Product-Enforcement Prototype Scope

## Purpose

Verify which intended Agentic MVP Launcher controls are supported by the selected Visual Studio Code and GitHub Copilot environment.

P0 produces compatibility evidence. P0 does not implement framework
components.

## Prototype Sessions

- P0A: Environment and harness inventory
- P0B: Tool and hook coverage
- P0C: Sandbox enforcement
- P0D: Approval binding and replay rejection
- P0E: Local audit and trusted checkpoint feasibility
- P0F: Telemetry and context-sizing feasibility
- P0G: MCP boundary verification, only if MCP is required
- P0H: Compatibility synthesis and specification readiness

## Allowed

- Disposable fixture files
- Local test scripts
- Prototype hook configurations
- Synthetic sandbox tests
- Synthetic approval records
- Local audit fixtures
- Sanitized compatibility evidence
- Read-only product and configuration inspection

## Prohibited

- Production access
- Azure state changes
- Azure DevOps pipeline execution
- Real repository pushes during prototype tests
- RBAC or permission changes
- Secret retrieval
- Real deployment activity
- Wiz changes
- Enterprise MCP server access
- Host Docker socket mounting
- Unsandboxed fallback for sandbox-required tests
- Modification of accepted ADRs
- Framework implementation
- Specification or implementation-ticket generation

## Evidence Classifications

Every tested control must be classified as one of:

- `VERIFIED`
- `PARTIALLY_VERIFIED`
- `EXTERNAL_CONTROL_REQUIRED`
- `UNSUPPORTED`
- `DEFERRED`
- `NOT_TESTED`

## Safety Rule

A prototype failure is evidence. Do not weaken a control or change the expected result merely to produce a passing test.