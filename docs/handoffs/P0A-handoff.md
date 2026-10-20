# P0A Handoff — Environment and Harness Inventory

**Date:** 2026-10-20  
**Status:** Inventory recorded; harness selection blocked on UI confirmation  
**Scope:** Read-only local inspection and first-party documentation; P0B and later work not started

## Selected operating environment

- **OS:** Microsoft Windows 11 Enterprise, version 10.0.22631, build 22631.
- **VS Code CLI:** 1.142.0, commit `6d62b1d5dace93a33b3a6d3eca1317bd5e15bcb1`, x64.
- **Copilot extension:** `GitHub.copilot` / `GitHub.copilot-chat` was not listed by
  `code --list-extensions --show-versions`. This identifies the CLI installation only and may
  not match the window hosting the current session. The installed
  `ms-azuretools.vscode-azure-github-copilot` 1.0.244 extension is a separate Azure extension,
  not the GitHub Copilot extension version.
- **Session Target / harness:** Not determined. Copilot is a candidate only; the actual selected
  target must be confirmed in the VS Code UI. Do not proceed as if Copilot or Local has been
  selected.
- **P0 hook prototype surface:** Undecided. Candidates are VS Code local agent sessions, Copilot
  CLI connected to VS Code, Copilot cloud agent, Copilot Shared harness, or another available
  Session Target. Hook coverage and execution environments differ; do not write P0B hook
  handlers until one target is explicitly selected and its event/payload contract is verified.
- The requested `prototypes/P0A-environment/manual-environment-inventory.md` was absent. The
  organizational-decision template contained no filled product baseline or policy answers.

## Feature availability

First-party VS Code documentation describes custom agents, explicit tool lists, hooks,
approval/permission modes, sandboxing, MCP, subagents, Agent Skills, and telemetry surfaces.
These are `AVAILABLE_UNTESTED` or `REQUIRES_PROTOTYPE` as listed in the
[capability matrix](../compatibility/harness-capability-matrix.yaml). None is `VERIFIED`.
Documentation support and a user setting are not evidence of effective enforcement.

Notable conditions:

- VS Code hooks are Preview and surface-specific. GitHub documents hooks for Copilot CLI and
  Copilot cloud agent; repository hook files use `.github/hooks/*.json`, while Copilot CLI can
  also load policy-, user-, repository-, settings-, and plugin-level hooks. Supported events
  and execution environments differ; the same hook JSON must not be assumed to provide
  identical coverage across surfaces.
- Copilot terminal sandbox is documented as off by default; outbound network is allowed by
  default. Windows requires the applicable September 8, 2026 security update, which was not
  checked. Copilot documents `/sandbox policy` for inspection of effective sandbox policy, but
  it was not run because the target remains unresolved.
- MCP has a separate sandbox/configuration boundary and no server was configured or contacted.
  Per-server MCP sandboxing is documented for macOS/Linux only and is unavailable on this
  Windows environment; the distinct session-level sandbox may cover local MCP processes.
- Subagent behavior and controls vary by harness; none was enabled or invoked.
- Agent Skills support is documented for Copilot, but no skill was installed or executed.
- Copilot Chat documents OTel traces, metrics, events, tool/model/token attributes, available
  cache-token fields, and connected parent/subagent spans. Environment availability and
  organizational approval remain unverified/pending. No exporter was configured by this P0A
  work; current effective environment configuration was not inspected. Telemetry prototype and
  export approval belong in P0F, not P0A. The accepted plan/ADR sequence separately defers
  provider-usage telemetry proof to M3A.
- The separate Copilot organization usage metrics dashboard requires its feature policy and
  administrator access; neither was checked.

## Organization restrictions

No effective organization or enterprise restriction was directly observed. No administrator
console or managed policy payload was accessed. Restrictions are **unknown**, not absent; see
[organization restrictions](../compatibility/organization-restrictions.md).

## Facts requiring manual UI verification

In the same VS Code window hosting the work:

1. Read the Session Target in the chat title, Agents-window watermark, or session-entry tooltip;
   record the exact selected harness.
2. Decide which P0 hook prototype surface is intended: VS Code local agent, Copilot CLI connected
   to VS Code, Copilot cloud agent, Copilot Shared harness, or another available target. Record
   the exact choice; do not start P0B or write hook handlers before this decision.
3. In Extensions, identify the installed GitHub Copilot/Copilot Chat version for that window.
4. Inspect the Agent Customizations UI for enabled tools and available Hooks/Skills controls;
   do not create or enable any.
5. Record the displayed approval/permission mode and whether organization-managed values are
   indicated. Do not change settings.
6. Determine whether a managed sandbox policy is displayed; do not run a denial test in P0A.
7. Confirm visible MCP and subagent availability/restrictions without configuring a server or
   enabling delegation.
8. Obtain sanitized confirmation of applicable organization policies (including telemetry and
   sandbox requirements) from an authorized source. Do not authenticate to enterprise systems
   or retrieve credentials.

## P0B prerequisites

- Resolve the actual session harness and the Copilot extension version in that window.
- Resolve required P0A UI and organization-policy facts.
- Explicitly select the P0 hook prototype surface before P0B. Use that surface's official,
  version-specific documentation to confirm supported events, payload schema, execution
  environment, and platform prerequisites; do not assume event/coverage parity between surfaces.
- For Windows CLI hook examples, verify the documented PowerShell 7.0+ prerequisite only if
  applicable to the selected runtime.
- Obtain separate authorization to start P0B. Keep hooks inactive until that authorization and
  P0B's explicit scope are established.
- Keep MCP and subagents disabled unless separately authorized; no MCP is required for P0B.

## Blockers

- Active target/harness unresolved.
- GitHub Copilot extension version in the active window unresolved.
- Organization-managed restrictions/effective policy unknown.
- Windows sandbox prerequisite and effective sandbox configuration unknown.
- Hook event/payload coverage not fully confirmed.
- P0 hook prototype surface has not been selected.

## Exact recommended next session

**`P0A-UI-VERIFICATION — Confirm the active VS Code Session Target, extension version, and
organization-managed feature/approval/sandbox restrictions without changing settings or
enabling features.`**

After that session, review the evidence and separately authorize P0B. Do not start P0B or any
later prototype in this handoff.

## Files delivered

- [Selected environment inventory](../compatibility/selected-environment.md)
- [Harness capability matrix](../compatibility/harness-capability-matrix.yaml)
- [Organization restrictions](../compatibility/organization-restrictions.md)
- [Unsupported or unverified controls](../compatibility/unsupported-or-unverified-controls.md)
- [P0A results](../../prototypes/P0A-environment/results.yaml)
- [This handoff](./P0A-handoff.md)

## Validation

Local fact gathering used `code --version`, `code --list-extensions --show-versions`, a
read-only Windows OS version query, and a narrowly filtered read of selected primitive VS Code
user setting values. No credentials or full environment dumps were read. All deliverables were
re-read and YAML/Markdown structural validation is reported in the completion response.
