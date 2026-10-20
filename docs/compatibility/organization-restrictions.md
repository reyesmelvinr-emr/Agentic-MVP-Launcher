# Organization Restrictions and Policy Evidence (P0A)

**As of:** 2026-10-20  
**Evidence boundary:** Local read-only inspection plus first-party product documentation. No
organization/enterprise administrator console or managed-policy payload was accessed.

## Observed restrictions

No effective organization restriction was directly observed during this inventory. This does
**not** establish that no restriction exists. The applicable Copilot organization, enterprise,
and VS Code managed settings are unknown.

| Control area | Local observation | Classification | Evidence / limitation |
|---|---|---|---|
| GitHub Copilot entitlement and feature policies | Not inspected | `REQUIRES_PROTOTYPE` | Organization settings were not accessed; official policy reference describes organization/enterprise control [1][2]. |
| VS Code managed AI settings | Not inspected | `REQUIRES_PROTOTYPE` | Effective managed policy cannot be inferred from user settings; VS Code documents enterprise-managed settings and configuration policy layers [3]. |
| Sandbox required by organization | Not inspected | `REQUIRES_PROTOTYPE` | No managed setting or effective sandbox status was read [3][4]. |
| Custom agent/hook/MCP restrictions | Not inspected | `REQUIRES_PROTOTYPE` | No managed-settings payload or admin policy surface was examined [1][3][5]. |
| Copilot usage metrics dashboard policy | Not inspected | `REQUIRES_PROTOTYPE` | Dashboard availability requires the Copilot usage metrics policy and appropriate organization/enterprise access [6]. |
| Copilot OTel export approval / managed telemetry configuration | Not inspected; approval status pending | `REQUIRES_PROTOTYPE` | VS Code documents organization-managed telemetry configuration; no effective managed setting or approved exporter/collector was inspected [3][7]. |
| Local user preferences | `chat.useHooks=true`; `github.copilot.chat.agent.enabled=true`; `chat.mcp.access=all`; `chat.mcp.gallery.enabled=true`; `chat.mcp.autostart=never` | `AVAILABLE_UNTESTED` | Values were read from user `settings.json`; they are not proof of effective policy, capability, or organizational permission. |
| GitHub Copilot extension in the CLI installation | Not listed | `UNAVAILABLE` | `code --list-extensions --show-versions` did not list `GitHub.copilot` or `GitHub.copilot-chat`; the active window may use a different installation. |

## Relevant documented organization control surfaces

1. GitHub provides Copilot policies at enterprise and organization scopes; the policy interface
   and availability depend on plan and administrative permissions [1][2].
2. VS Code documents enterprise-managed AI settings and managed defaults for permission
   levels. A managed value may differ from a user preference [3].
3. VS Code documents that administrators should use managed settings to require Copilot
   sandboxing; user-level setting presence is not sufficient evidence of an enforced policy [3][4].
4. The Copilot usage dashboard is an organization/enterprise administration surface and depends
   on its feature policy being enabled [6].
5. VS Code documents centrally mandated OTel export configuration through Copilot
   enterprise-managed settings. Whether any such configuration is applied or approved here is
   unknown [3][7].

These are available product controls, **not findings that the organization has enabled or
disabled any particular setting**.

## Sources

1. GitHub Docs, [GitHub Copilot policies for enterprises and organizations](https://docs.github.com/en/copilot/concepts/enterprise/policies).
2. GitHub Docs, [Managing policies and features for GitHub Copilot in your organization](https://docs.github.com/en/copilot/how-tos/administer-copilot/manage-for-organization/manage-policies).
3. VS Code Docs, [Manage AI settings in enterprise environments](https://code.visualstudio.com/docs/enterprise/manage-ai-settings).
4. VS Code Docs, [Sandbox Copilot agent sessions](https://code.visualstudio.com/docs/agents/run/agent-sandboxing).
5. GitHub Docs, [Enterprise managed settings](https://docs.github.com/en/copilot/reference/enterprise-administrators/enterprise-managed-settings).
6. GitHub Docs, [Viewing the Copilot usage metrics dashboard](https://docs.github.com/en/copilot/how-tos/administer-copilot/view-usage-and-adoption).
7. VS Code Docs, [Monitor agent usage with OpenTelemetry](https://code.visualstudio.com/docs/agents/guides/monitoring-agents).

## Manual verification required

The next P0A session must establish the account's applied organization policy using the VS Code
UI and an authorized admin-provided policy view or confirmation. Do not request credentials,
change settings, or access enterprise systems as part of this inventory. Record only sanitized
policy names/statuses needed to determine capability availability.
