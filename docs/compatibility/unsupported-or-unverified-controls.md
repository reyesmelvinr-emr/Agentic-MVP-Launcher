# Unsupported or Unverified Controls (P0A)

**As of:** 2026-10-20  
**Boundary:** Inventory only; no control was enabled, weakened, bypassed, or tested.

| Control / fact | Classification | Current evidence | What remains unknown / follow-up |
|---|---|---|---|
| Active Session Target / harness | `REQUIRES_PROTOTYPE` | CLI facts do not expose the active UI target; Copilot is only a candidate. | Inspect VS Code chat title / Agents-window target watermark or session tooltip. Record exact harness and target. |
| Copilot extension version in active VS Code window | `REQUIRES_PROTOTYPE` | CLI extension inventory did not list `GitHub.copilot` or `GitHub.copilot-chat`; command may target a different installation than this session. | Inspect Extensions UI in the same VS Code window. Do not substitute the Azure Copilot extension version. |
| Organization and enterprise managed policy | `REQUIRES_PROTOTYPE` | No admin console or effective managed-settings view inspected. | Obtain sanitized admin confirmation / inspect the policy surface available to the operator. |
| Windows sandbox OS prerequisite | `REQUIRES_PROTOTYPE` | Windows 11 Enterprise build 22631 recorded; no update/KB state checked. Docs specify the applicable September 8, 2026 Windows security update [1]. | Confirm prerequisite is installed via approved UI/IT information before sandbox prototype. |
| Effective sandbox state and file/network policy for this session | `REQUIRES_PROTOTYPE` | No sandbox setting values or effective policy were inspected; no denial tests run. Docs state sandbox is off by default and network is allowed by default [1]. Copilot documents `/sandbox policy` as a local effective-policy report [1]. | Confirm the harness, then inspect the report without changing settings. P0C later must prove behavior in a disposable fixture. |
| P0 prototype hook target and exact hook coverage | `REQUIRES_PROTOTYPE` | Multiple surfaces have distinct hook behaviors: VS Code local agent sessions, Copilot CLI connected to VS Code, Copilot cloud agent, Copilot Shared harness, or another Session Target. GitHub documents repository hooks in `.github/hooks/*.json` and additional Copilot CLI hook sources; event support and execution environment vary by surface [2][3][4][5]. | Explicitly choose the P0 prototype surface, then verify its events, payloads, execution environment, and platform prerequisites from the matching versioned docs. Do not write P0B hook handlers or assume the same JSON has identical coverage across surfaces before that decision and separate authorization. |
| Windows Copilot CLI hook runtime prerequisite | `REQUIRES_PROTOTYPE` | GitHub's CLI setup guide says Windows examples require PowerShell 7.0+ in PATH; version not checked [5]. This is specific to CLI examples. | If applicable to the selected runtime, confirm the approved PowerShell version. No hook command was run. |
| MCP session-level sandbox | `AVAILABLE_UNTESTED` | Copilot session sandbox may cover locally launched/managed MCP servers when enabled; no server config or invocation inspected [1]. | Do not configure MCP during P0A. If required, establish source/version, identity, credentials, network, logging, and distinct sandbox coverage under P0G. |
| MCP per-server sandbox on Windows | `UNAVAILABLE` | VS Code documents per-server sandboxing for stdio MCP servers on macOS and Linux, not Windows [6]. | Do not plan per-server MCP sandbox enforcement on this Windows environment; if MCP is required, assess a separately verified external boundary. |
| MCP server configuration and independent policy | `AVAILABLE_UNTESTED` | User preferences show MCP access/gallery settings; no server config or invocation inspected. MCP settings are separate from terminal controls [1][6]. | Do not configure MCP during P0A. If required, establish source/version, identity, credentials, network, logging, and distinct sandbox coverage under P0G. |
| Subagent enablement / restriction in active target | `REQUIRES_PROTOTYPE` | Product documentation describes harness-specific subagents; active target and effective configuration unknown [7]. | Confirm the harness and controls; keep disabled unless separately approved. |
| Agent Skills runtime discovery/loading | `AVAILABLE_UNTESTED` | VS Code documents Copilot Skill support; no local skill was created or invoked [8]. | Determine supported locations for selected target later; do not create operational skills in P0A. |
| Copilot Chat OpenTelemetry monitoring availability in this environment | `AVAILABLE_UNTESTED` | VS Code documents traces, metrics, events, tool spans, requested/resolved model attributes, token usage, cache-token fields when available, and connected parent/subagent spans [9]. No OTel settings, environment variables, effective managed configuration, or runtime state were inspected. | Inventory the OTel settings/managed configuration only when authorized; do not enable or export during P0A. Actual availability and organizational approval remain unverified/pending. Schedule feasibility/telemetry prototype in P0F. |
| OTel exporter / current effective export configuration | `REQUIRES_PROTOTYPE` | OTel can be activated by extension settings, a local DB span exporter, or environment variables such as `OTEL_EXPORTER_OTLP_ENDPOINT`; managed settings can mandate export [9]. This P0A work configured no exporter, but current environment configuration was not inspected. | Do not claim the entire environment is unconfigured based only on this session. Verify effective settings/policy in P0F or an authorized inventory review before testing. |
| Copilot org usage dashboard and export | `REQUIRES_PROTOTYPE` | Feature policy and organization access requirements documented; neither inspected [10]. | Confirm policy/role with an authorized administrator. No export or telemetry integration in P0A. |
| VS Code editor telemetry settings | `REQUIRES_PROTOTYPE` | Product docs describe editor telemetry controls, but local telemetry values were not inspected [11]. | Inspect approved UI settings only if needed; no external telemetry export. |
| Repository hooks, custom agents, Skills, workspace MCP config | `UNAVAILABLE` | No `.github/hooks/`, `.github/agents/`, `.github/skills/`, or workspace MCP config was found in this repository. | This only describes repository-local files; it does not establish absence of user-level or provider-provided features. |

## P0 boundary

No hook was created or run, no MCP server was configured or contacted, no subagent was enabled
or invoked, no sandbox enforcement test was attempted, and no telemetry was exported. These
remain future, separately authorized prototype work; this record does not start P0B or later.

## Sources

1. VS Code Docs, [Sandbox Copilot agent sessions](https://code.visualstudio.com/docs/agents/run/agent-sandboxing).
2. VS Code Docs, [Configure agent hooks in Visual Studio Code](https://code.visualstudio.com/docs/agent-customization/hooks).
3. VS Code Docs, [Hooks reference](https://code.visualstudio.com/docs/agents/reference/hooks-reference).
4. GitHub Docs, [GitHub Copilot hooks reference](https://docs.github.com/en/copilot/reference/hooks-reference).
5. GitHub Docs, [Using hooks with GitHub Copilot CLI](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/use-hooks).
6. VS Code Docs, [Manage and troubleshoot MCP servers](https://code.visualstudio.com/docs/agents/run/manage-mcp-servers).
7. VS Code Docs, [Use subagents in Visual Studio Code](https://code.visualstudio.com/docs/agents/run/subagents).
8. VS Code Docs, [Use Agent Skills in VS Code](https://code.visualstudio.com/docs/agent-customization/agent-skills).
9. VS Code Docs, [Monitor agent usage with OpenTelemetry](https://code.visualstudio.com/docs/agents/guides/monitoring-agents).
10. GitHub Docs, [Viewing the Copilot usage metrics dashboard](https://docs.github.com/en/copilot/how-tos/administer-copilot/view-usage-and-adoption).
11. VS Code Docs, [Telemetry](https://code.visualstudio.com/docs/configure/telemetry).
