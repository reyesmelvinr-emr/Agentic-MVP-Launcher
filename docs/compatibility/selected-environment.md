# Selected Environment Compatibility Inventory (P0A)

**Scope of this document.** This is an evidence-based inventory of locally observed product
facts and first-party VS Code / GitHub Copilot documentation. Product documentation establishes
documented support only; it does not establish effective local behavior or policy enforcement.
No enforcement behavior was tested. No authentication, configuration changes, hook/agent
creation, or enterprise-system access was performed.

**Research date:** 2026-10-20 (UTC+8). Docs sites change frequently; re-verify URLs/headings if
this file is reused after a significant time gap. Local facts were collected read-only after
approval in this session.

## Classification labels

The capability matrix uses only the requested classifications:
`AVAILABLE_UNTESTED`, `UNAVAILABLE`, `ORGANIZATION_RESTRICTED`, `REQUIRES_PROTOTYPE`, and
`NOT_APPLICABLE`. No capability is `VERIFIED`: local enforcement behavior was not demonstrated.
In this evidence inventory, product documentation is evidence of documented support, not of
effective availability or enforcement.

## 0. Local environment evidence and unresolved UI facts

Read-only local command output:

| Item | Observation | Evidence / limitation |
|---|---|---|
| VS Code | 1.142.0, commit `6d62b1d5dace93a33b3a6d3eca1317bd5e15bcb1`, x64 | `code --version`; reports the CLI installation, not necessarily the VS Code window hosting this conversation. |
| Operating system | Microsoft Windows 11 Enterprise, version 10.0.22631, build 22631 | `Get-CimInstance Win32_OperatingSystem`, selecting only caption/version/build. |
| GitHub Copilot extension | Not listed by `code --list-extensions --show-versions`; no `GitHub.copilot` or `GitHub.copilot-chat` version was identified | The VS Code window/session instance could differ from the CLI installation. Do not infer the session's extension version from this result. |
| Similar-named installed extension | `ms-azuretools.vscode-azure-github-copilot` 1.0.244 | This is the Azure GitHub Copilot extension, not evidence of the GitHub Copilot Chat extension version. |
| Session Target / active harness | Not identified from read-only CLI or settings evidence | UI inspection required. The conversation environment alone does not establish the VS Code UI's selected target. |
| User settings observed | `chat.useHooks=true`; `github.copilot.chat.agent.enabled=true`; `chat.mcp.access=all`; `chat.mcp.gallery.enabled=true`; `chat.mcp.autostart=never`; `chat.agent.maxRequests=100` | Read from the user's VS Code `settings.json`. These are user-level preferences only; they do not prove effective managed settings, active features, or enforcement. |
| Workspace settings | No workspace `.vscode/settings.json` found | Checked at the repository root only. |
| Repository customizations | No `.github/hooks/`, `.github/skills/`, `.github/agents/`, or `.vscode/settings.json` found in this repository | Read-only file inventory. This does not establish absence of user-level or extension-provided customizations. |
| Organization restrictions | Unknown | No managed policy/effective settings or organization admin view was inspected. |

**Candidate, not selection:** the Copilot harness is a candidate because the current product
context is Copilot in VS Code. The actual Session Target remains
`REQUIRES_PROTOTYPE`/manual UI verification. VS Code documentation says the active harness is
shown in the chat title, Agents-window input watermark, or session-entry tooltip; see
[Choose an agent harness](https://code.visualstudio.com/docs/agents/run/agent-harnesses),
"Identify your current harness."

No feature is marked `VERIFIED`. Local feature operation and effective policy would require
direct demonstration and are outside this read-only inventory.

---

## 1. Agent harness / session target concept

**Source:** "Choose an agent harness" — VS Code docs —
https://code.visualstudio.com/docs/agents/run/agent-harnesses

- AVAILABLE_UNTESTED — "VS Code supports multiple agent harnesses, each with its own tools,
  customizations, and workflows. An agent harness coordinates the language model, context, and
  tools that an agent uses to complete your task." (intro)
- AVAILABLE_UNTESTED — Five harness/target options are compared in the "Compare agent harnesses" table:
  **Copilot** (runs via the Copilot SDK / shared Agent Host, can continue in the background),
  **Local** (VS Code's built-in harness, runs in the extension host, tied to the editor window),
  **Claude** (Anthropic Claude Agent SDK integrated into VS Code), **Codex** (OpenAI Codex via
  extension or experimental Agent Host integration), and **Cloud** (an execution target, not a
  single harness — "An available provider harness runs on provider-managed remote
  infrastructure... The session does not use local editor context.").
- AVAILABLE_UNTESTED — Under "Select a harness": the **Session Target** UI control must be selected
  "before you submit the first prompt for a session," from the Chat view or Agents window;
  "The control only lists targets whose requirements are met in the current window."
- AVAILABLE_UNTESTED — Under "Choose a built-in agent": available built-in agents depend on harness.
  For **Copilot**: Ask (read-only), Agent (implements changes), Plan (`/plan`, research +
  implementation plan). For **Local**: Ask, Agent, Plan (same descriptions). "Switching agents
  doesn't change the harness." Custom agents appear in the same picker.
- AVAILABLE_UNTESTED — Under "Identify your current harness": the active harness is shown "as a prefix
  in the chat title" (editor window), as the chat input watermark (Agents window), or via a
  tooltip on hover over the session entry (Agents window).
- AVAILABLE_UNTESTED — Under "Requirements and setup / Copilot": "Copilot harness sessions use the same
  GitHub authentication context as chat in VS Code, including your GitHub Enterprise account
  when configured." Cites GHE.com managed-account setup separately
  (https://docs.github.com/en/copilot/managing-copilot/configure-personal-settings/using-github-copilot-with-an-account-on-ghecom).
  Also notes: "Copilot harness sessions don't have access to every built-in or extension-provided
  tool," and harness-specific customizations such as hooks "follow the selected harness."

---

## 2. Custom agent files: locations and supported properties

**Sources:**
- "Custom agents in VS Code" — https://code.visualstudio.com/docs/agent-customization/custom-agents
  (redirect target of `.../docs/copilot/customization/custom-chat-modes`)
- "Custom agent file reference" — https://code.visualstudio.com/docs/agents/reference/custom-agent-file-reference
- "About custom agents" — https://docs.github.com/en/copilot/concepts/agents/copilot-cli/about-custom-agents

### 2.1 Locations

- AVAILABLE_UNTESTED — Custom agent files are created via the **Agent Customizations editor** at
  Workspace or User scope. The documented VS Code locations are workspace `.github/agents/`
  (`.agent.md` or `.md` files) and User (Copilot) `~/.copilot/agents/` (`.agent.md` or `.md`
  files). In this Windows environment, `~` represents the user's home directory, so the default
  user path is `%USERPROFILE%\.copilot\agents\`. The active harness determines which location
  applies.
- AVAILABLE_UNTESTED — GitHub documents organization-level custom agents stored in designated
  `.github` or `.github-private` repositories. Whether these are visible to this VS Code session
  depends on organization setup and harness integration; neither was inspected here.
- AVAILABLE_UNTESTED — "Files in `.claude/agents` use the Claude agent format" (noted separately under
  "Harness compatibility" in the file reference page — a distinct location/format from VS Code's
  `.agent.md`).

### 2.2 Supported FrontMatter fields (from "Custom agent file reference")

| Field | Type | Notes (as documented) |
|---|---|---|
| `name` | String | Agent name in UI; defaults to file name |
| `description` | String | What the agent does / when to use it |
| `argument-hint` | String | Hint text for expected input; shown in Local chat input |
| `tools` | String or array | Tools/tool sets available; names depend on harness |
| `agents` | Array of strings | Custom agents available as subagents (Local); `['*']`/omit = all, `[]` = none |
| `model` | String or array | Model(s) to use in Copilot harness, in preference order |
| `reasoning-effort` | String | Copilot harness: `low`, `medium`, `high`, `xhigh`, or `max`; availability depends on model |
| `user-invocable` | Boolean | Whether agent appears in agents dropdown (Local/Copilot); default `true` |
| `disable-model-invocation` | Boolean | Prevents other agents from invoking as subagent; default `false` |
| `infer` | Boolean | **Deprecated** — use `user-invocable` + `disable-model-invocation` instead |
| `target` | String | `vscode` or `github-copilot` — target environment, not the harness selector |
| `mcp-servers` | Object | MCP server configs for custom agents in **Copilot cloud agent** only; not used by VS Code custom agents |
| `handoffs` | Array of objects | Suggested transitions to another agent after a response (Local only) |
| `hooks` (Preview) | Object | Hook commands scoped to this agent in Local; requires `chat.useHooks` + trusted workspace |

- AVAILABLE_UNTESTED — "All fields in this table are optional."
- AVAILABLE_UNTESTED — Under "Harness compatibility": "The selected agent harness determines which
  fields and tool names take effect. A shared file location does not imply identical behavior
  across harnesses." Specifically:
  - **Local** supports VS Code tool names, subagent configuration, handoffs, and
    agent-scoped hooks.
  - **Copilot** supports custom agent configuration "through its runtime," referencing
    GitHub's own docs (https://docs.github.com/en/copilot/concepts/agents/copilot-cli/about-custom-agents)
    for field-specific guidance; "Local handoffs and agent-scoped hooks do not apply."
  - **Claude** uses its own agent format and runtime behavior.
  - **Copilot cloud agent** has a separate configuration reference
    (https://docs.github.com/en/copilot/reference/custom-agents-configuration); the
    `target: github-copilot` frontmatter value refers to that target, "not the Copilot harness
    selected in VS Code."
- AVAILABLE_UNTESTED — Tools field: "In Local, `tools` can include built-in tools, tool sets, MCP
  tools, and tools contributed by extensions. To include all tools from an MCP server, use
  `<server name>/*`." If `tools` is omitted in Local, tools enabled in the tools picker are used;
  unavailable tools are ignored. "For Copilot and other harnesses, check tool availability
  before reusing a Local agent's tool list" (links to
  https://code.visualstudio.com/docs/agents/run/tools).
- AVAILABLE_UNTESTED — Subagent configuration: restrict a coordinator to named workers via
  `tools: ['agent']` plus `agents: ['Researcher', 'Implementer']`; "Agent names are
  case-sensitive." Explicitly listing an agent in `agents` permits invocation even if that
  worker sets `disable-model-invocation: true`. Nested delegation requires
  `chat.subagents.allowInvocationsFromSubagents`.
- AVAILABLE_UNTESTED — Custom agents also appear in the shared agent picker alongside built-in agents
  (per the custom-agents page, "Custom agents appear in the agent picker alongside the
  built-in agents").

---

## 3. Explicit tool lists / tool control (auto invocation controls)

**Source:** "Use tools with agents" — https://code.visualstudio.com/docs/agents/run/tools

- AVAILABLE_UNTESTED — "By default, the agent chooses from the enabled tools based on your request and
  the current context. You don't need to name or select each tool before you start"
  ("Use tools in a request").
- AVAILABLE_UNTESTED — Explicit tool reference: "Type `#` in the chat input to reference a specific
  tool or tool set... Use a tool set, such as `#web`, to make a group of related tools
  available" ("Reference a tool explicitly").
- AVAILABLE_UNTESTED — "Manage available tools": for Copilot harness sessions, tool availability is
  managed from the Agent Customizations editor → **Tools** tab; the selection "persists in the
  active user profile, rather than applying to only one request." Tools are organized into
  collapsible, searchable groups; a group checkbox can enable/disable every tool in the group.
  "All client-side tools are available to the Copilot harness by default."
- AVAILABLE_UNTESTED — For a reusable/explicit tool selection tied to a role, the docs point to
  specifying `tools` in a custom agent file (cross-referenced from §2 above) and, for Local, to
  creating a **tool set** (https://code.visualstudio.com/docs/agent-customization/tool-sets).
- AVAILABLE_UNTESTED — Auto-approval / auto-invocation of tool calls (as opposed to tool *availability*)
  is governed by the **approvals** system, not the tools page — see §5 below for
  `chat.tools.eligibleForAutoApproval`, `chat.tools.global.autoApprove`,
  `chat.tools.urls.autoApprove`, and `chat.tools.terminal.autoApprove`.

---

## 4. Hooks

**Source:** "Configure agent hooks in Visual Studio Code (Preview)" —
https://code.visualstudio.com/docs/agent-customization/hooks

### 4.1 Status and harness ownership

- AVAILABLE_UNTESTED — Page title/status: "Configure agent hooks in Visual Studio Code (**Preview**)."
  Explicit caveat: "The VS Code hooks experience is in Preview. Individual provider
  implementations might have a different lifecycle status. For example, hooks in the Copilot SDK
  are generally available."
- AVAILABLE_UNTESTED — "The selected agent harness determines which hook implementation runs... The
  harness owns the hook lifecycle and payloads." Some harnesses discover the *same hook files*
  (e.g., `.github/hooks/*.json`, `.claude/settings.json`) but "This file compatibility does not
  make their behavior identical. Supported events, event names, matchers, command properties,
  tool names, payloads, and output decisions can differ."
- AVAILABLE_UNTESTED — "Copilot harness sessions use the same SDK hook implementation as Copilot CLI.
  Use the Copilot CLI sections of the GitHub reference for runtime configuration and payloads,
  but verify that the event is available in the selected VS Code version."
- AVAILABLE_UNTESTED — Migration caution: before switching harnesses for an existing hook workflow,
  confirm the destination harness discovers the file; compare supported events/command
  properties; check tool names/input shapes; validate transcript-reading scripts ("Transcript
  formats are not a stable cross-harness API"); test output decisions (e.g., blocking a tool
  call); confirm where the hook command runs and what it can access.

### 4.2 Creating/managing hooks (Local harness)

- AVAILABLE_UNTESTED — Select harness first via **Session Target**, then open **Agent Customizations
  editor → Hooks**, or run **Chat: Configure Hooks** (Command Palette), or type `/hooks` in chat
  (Local or Copilot).
- AVAILABLE_UNTESTED — AI-assisted hook generation: `/create-hook <description>` (Local only) or
  **Chat: Generate Hook** (Command Palette); "generates hooks in `.github/hooks/`." "Review the
  result against the destination harness reference before you use it."

### 4.3 Local hook file locations (implementation locations)

| Scope | File location | Notes |
|---|---|---|
| Workspace | `.github/hooks/*.json` | Native VS Code or Copilot-compatible hook files; documented by GitHub's CLI hook guide |
| Workspace, Claude format | `.claude/settings.json`, `.claude/settings.local.json` | Requires `chat.useClaudeHooks` (off by default) |
| User, Copilot CLI | `%USERPROFILE%\.copilot\hooks\*.json` | If `COPILOT_HOME` is set, use `%COPILOT_HOME%\hooks\*.json` |
| User, Local | `~/.copilot/hooks/*.json` | VS Code docs list this as available across Local harness sessions |
| User, Claude format | `~/.claude/settings.json` | Requires `chat.useClaudeHooks` |
| Custom agent | `hooks` key in `.agent.md` frontmatter | Runs only for that custom agent in Local harness |
| Plugin | `hooks.json` or `hooks/hooks.json` | Format depends on plugin; see Agent plugin reference |

- AVAILABLE_UNTESTED — `chat.useHooks` controls Local hook execution, **on by default**; workspace hook
  files are subject to Workspace Trust. `chat.hookFilesLocations` can add/disable Local hook
  locations (example shows enabling a custom folder, a single file, or disabling
  `.claude/settings.json`).
- AVAILABLE_UNTESTED — Example minimal hook file format (`.github/hooks/audit.json`):
  ```json
  { "hooks": { "PreToolUse": [ { "type": "command", "command": "node .github/hooks/log-tool-use.cjs" } ] } }
  ```
  Demonstrated event: `PreToolUse` fires before a tool invocation and receives JSON via stdin
  including `timestamp` and `tool_name`.
- AVAILABLE_UNTESTED — Local hook file formats recognized: native VS Code (PascalCase events, no numeric
  `version`), Copilot format (numeric `version`, lower-camel-case events, mapped into Local
  schema), and Claude format (`settings.json`/`settings.local.json`; Local "ignores matcher
  values, so every command for the event runs").
- AVAILABLE_UNTESTED — The dedicated [Local hooks reference](https://code.visualstudio.com/docs/agents/reference/hooks-reference)
  defines Local harness events and payloads. Its event set includes `PreToolUse`, `PostToolUse`,
  `UserPromptSubmit`, `SessionStart`, `Stop`, `SubagentStart`, `SubagentStop`, and `PreCompact`.
  Hooks receive common JSON fields (`timestamp`, optional `cwd`, optional `session_id`,
  `hook_event_name`, optional `transcript_path`) plus event-specific fields; the reference
  documents output fields and exit-code semantics. This is the **Local harness** schema, not a
  guarantee of the Copilot harness schema.
- AVAILABLE_UNTESTED — VS Code directs Copilot-harness sessions to GitHub's [Copilot CLI hooks
  reference](https://docs.github.com/en/copilot/reference/hooks-reference) because the Copilot
  harness uses the same SDK hook implementation. The documented Copilot CLI event names are
  `agentStop`, `errorOccurred`, `notification`, `permissionRequest`, `postToolUse`,
  `postToolUseFailure`, `preCompact`, `preToolUse`, `sessionEnd`, `sessionStart`,
  `subagentStart`, `subagentStop`, and `userPromptSubmitted`. GitHub's reference provides
  event-specific input payloads (for example, `sessionId` and millisecond `timestamp`,
  `toolName`/`toolArgs`, prompt, permission, and subagent fields). The VS Code hooks page still
  requires checking event availability for the selected VS Code version; do not assume every
  CLI event is exposed in the Copilot Session Target.
- AVAILABLE_UNTESTED — GitHub's [Copilot CLI hook setup guide](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/use-hooks)
  documents repository hooks in `.github/hooks/*.json` and Windows user hooks in
  `%USERPROFILE%\.copilot\hooks\*.json` (or `%COPILOT_HOME%\hooks\*.json` when configured).
  Its Windows examples require PowerShell 7.0+ in `PATH`; that runtime version was not checked.

### 4.4 Platform prerequisites for hooks

- AVAILABLE_UNTESTED — VS Code Local workspace hooks require Workspace Trust and
  `chat.useHooks`; GitHub's Copilot CLI Windows examples require PowerShell 7.0+. That CLI
  requirement must not be generalized to another hook runtime without checking that runtime's
  documentation. If hook commands run inside an enabled agent sandbox, the selected harness's
  sandbox prerequisites (§6.2) also matter. No hook was executed in this inventory.

### 4.5 Hook coverage differs by Session Target — P0 target is undecided, P0B handlers blocked

**Sources:** "About hooks for GitHub Copilot" —
https://docs.github.com/en/copilot/concepts/agents/hooks ;
"GitHub Copilot hooks reference" — https://docs.github.com/en/copilot/reference/hooks-reference ;
"Customize agent workflows with hooks" (Copilot cloud agent how-to; the URL path in this task's
brief, `.../use-copilot-agents/coding-agent/use-hooks`, redirected to
`.../how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/use-hooks`) —
https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/use-hooks.

- **Repository hook location confirmed.** All three sources state hook configuration files live
  at `.github/hooks/*.json` in the repository (concepts page: "stored in your repository at
  `.github/hooks/*.json`. These apply whenever Copilot agents are used in the repository"; how-to
  page: file "**must be present** on your repository's default branch to be used by Copilot cloud
  agent"). This matches §4.3's `.github/hooks/*.json` row, but "Copilot agents" here refers to
  **Copilot CLI and Copilot cloud agent specifically** — the GitHub docs do not claim this covers
  every VS Code Session Target (see below).
- **Hooks are documented for only two Copilot surfaces, not all Session Targets.** The concepts
  page states: "Hooks are available for use with: Copilot cloud agent on GitHub. GitHub Copilot
  CLI in your terminal." The reference page is explicit that it covers only those two: "Hooks are
  supported in two Copilot surfaces: Copilot CLI and Copilot cloud agent... the execution
  environment and the set of events that can fire differ." Neither GitHub page documents behavior
  for VS Code's **Local** harness, **Claude**, **Codex**, or a **"Copilot Shared harness"** —
  §4.1's VS Code source is the only first-party evidence for Local-harness hook behavior, and it
  is explicitly Preview-status and harness-owned ("the selected agent harness determines which
  hook implementation runs"). No first-party page found in this pass documents a "Copilot Shared
  harness" Session Target by that name; it must not be assumed to inherit either the CLI/cloud-agent
  hook behavior above or the VS Code Local-harness behavior in §4.3–4.4 without separate
  verification.
- **CLI hook configuration layers confirmed, with an ordering discrepancy to flag.** The
  hooks-reference page states Copilot CLI hooks are "loaded from the following sources in order
  (policy, then user, then project, then plugins) and combined," but its own bulleted list
  enumerates, in this order: policy-level (`/etc/github-copilot/policy.d/*.json`,
  `C:\ProgramData\GitHub\Copilot\policy.d\*.json`, or Windows registry
  `HKLM\Software\Policies\GitHub\Copilot`), **repository**-level (`.github/hooks/*.json`),
  **user**-level (`~/.copilot/hooks/` or `%USERPROFILE%\.copilot\hooks\`, or `$COPILOT_HOME/hooks/`
  if set), inline `hooks` block in repository **settings**
  (`.github/copilot/settings.json`/`.local.json`, also cross-reading `.claude/settings.json`/
  `.local.json`), inline `hooks` block in user-level **settings**
  (`~/.copilot/settings.json`), and **plugin**-contributed hooks (`hooks.json` per plugin). "When
  the same event appears in multiple sources, all hook entries from all sources are run." The
  prose summary ("policy, user, project, plugins") and the bullet order (policy, repository, user,
  settings, settings, plugin) do not match term-for-term; both are quoted here rather than
  reconciled, since this document is not authoritative over GitHub's docs. Either way, the
  practical point stands: **policy, user, repository, settings, and plugin are five distinct
  configuration layers that are all loaded and combined**, not a single location.
- **Execution environment and supported events differ sharply between CLI and cloud agent**, per
  the reference page's own framing ("Throughout this article, behavior that differs... is called
  out"): Copilot **CLI** hooks "run on the developer's local machine in the same shell as the
  CLI... All hook events described in this article are supported." Copilot **cloud agent** hooks
  "run inside the ephemeral Linux sandbox that cloud agent provisions for each job. The sandbox is
  non-interactive, has a constrained network, and is destroyed when the job ends. A subset of
  events fires, and only `bash` (or `command`) entries are honored" (`powershell` entries are
  ignored). Cloud agent also has no policy-hook layer ("Policy hooks are not supported under
  Copilot cloud agent"), no user-level hook files, no `settings.json`/`config.json`, and no
  installed plugins — "the only hook configuration that exists by default is `.github/hooks/*.json`
  inside the cloned repository." Cloud agent's filesystem is ephemeral (hook-written files are
  discarded at job end) and outbound network is firewall-restricted to GitHub/Copilot hostnames by
  default.
- **Conclusion for this prototype.** §0 and §1 already establish that the VS Code **Session
  Target is unresolved** (`REQUIRES_PROTOTYPE`) and that harnesses "can differ" in supported
  events, payloads, and command properties even when they read the same `.github/hooks/*.json`
  file (§4.1). Combined with the CLI-vs-cloud-agent differences just cited, and the complete
  absence of first-party documentation for Local/Claude/Codex/"Shared" harness hook coverage from
  GitHub's own hooks docs: **hook coverage must not be assumed equivalent across VS Code local
  agent sessions, Copilot CLI connected to VS Code, Copilot cloud agent, a "Copilot Shared
  harness," or any other undetermined Session Target.** Because the P0 prototype target has not
  been selected, **P0B hook handlers must not be written** until a Session Target is explicitly
  chosen and its specific hook event set, configuration-layer precedence, and execution
  environment are verified against the harness-appropriate reference (VS Code Local reference in
  §4.3, or GitHub's CLI/cloud-agent reference above) — writing a handler against the wrong
  surface's event names, payload shape, or `bash`/`powershell`/`command` field risks a handler
  that silently never fires, or fires with a different payload than expected.

---

## 5. Approvals (and auto-invocation controls)

**Source:** "Manage approvals and permissions" — https://code.visualstudio.com/docs/agents/run/approvals

- AVAILABLE_UNTESTED — Two "security layers": **Approvals** ("determine whether an action runs
  automatically or requires your confirmation") and **Sandboxing** ("restricts the file system
  and network resources that agent terminal commands can access, even after the commands are
  approved").
- AVAILABLE_UNTESTED — Mechanism/setting summary table:

  | Mechanism | Key setting |
  |---|---|
  | Permission levels | `chat.permissions.default` |
  | Tool approval | `chat.tools.eligibleForAutoApproval` |
  | URL approval | `chat.tools.urls.autoApprove` |
  | Terminal approval | `chat.tools.terminal.autoApprove` |
  | Sandboxing | `chat.agent.sandbox.enabled` |

- AVAILABLE_UNTESTED — Permission levels (Copilot harness): **Manual permissions** (default) — "Uses
  your tool, URL, and terminal approval settings. Actions that are not auto-approved require
  your confirmation"; **Assisted permissions** — "Uses an LLM judge to assess each tool call.
  Calls that the judge does not approve require your confirmation" (requires
  `chat.assistedPermissions.enabled`; requires Folder isolation; not available for Local
  harness); **Allow all** — "Runs all tool calls without confirmation." New sessions use
  `chat.permissions.default`; "An organization can set a different managed default for Copilot
  harness sessions," and can also hide Assisted permissions / Allow all via enterprise-managed
  settings (links to
  https://code.visualstudio.com/docs/enterprise/manage-ai-settings#_manage-permission-levels).
- AVAILABLE_UNTESTED — Explicit caution: "Sandboxing is independent of the permission level. Allow all
  and Autopilot skip approval prompts, but an enabled sandbox still restricts terminal file
  system and network access. The sandbox doesn't cover every tool, so use tool and URL approvals
  to control other actions with Manual permissions."
- AVAILABLE_UNTESTED — **Autopilot**: "Autopilot is an agent mode, not a permission level. Select it
  from the mode picker in the chat input to let the agent work autonomously... Auto-approves all
  tools, like Allow all. Retries when it encounters errors. Responds automatically to questions
  that would otherwise block progress." Note: "For Local harness sessions: Autopilot is a
  permission level rather than an agent mode." **Advanced Autopilot (Preview)**: delegates
  completion-decision to a separate fast model each turn; gated by
  `chat.autopilot.advanced.enabled`.
- AVAILABLE_UNTESTED — Claude harness permission modes: **Edit automatically**, **Request approval**,
  **Plan**. Caution: `github.copilot.chat.claudeAgent.allowDangerouslySkipPermissions` "bypasses
  all permission checks. Use it only in an isolated sandbox environment without internet access."
- AVAILABLE_UNTESTED — Codex approval presets (direct integration): **Default Permissions** (read/edit
  workspace files, run routine local commands; asks before internet/external-resource access),
  **Auto-Review** (same access, but approval requests go to an automatic reviewer instead of the
  user), **Full Access** ("gives Codex unrestricted disk and network access").
- AVAILABLE_UNTESTED — Global auto-approval: `chat.tools.global.autoApprove` auto-approves tools across
  all workspaces; `/yolo` or `/autoApprove` bypass approvals for the current session only
  (Local/Copilot CLI); `/disableYolo` / `/disableAutoApprove` restore defaults. Caution: "Global
  auto-approval removes confirmation prompts in every workspace."
- AVAILABLE_UNTESTED — Tool approval: command **Chat: Manage Tool Approval** reviews/configures
  approvals, grouped by source (MCP server/extension); approval types are **Pre-approval**
  ("Runs the tool without a confirmation dialog") and **Post-approval** ("Adds the tool result to
  the chat context without review. Use caution with external data that might contain prompt
  injection."). Setting a tool to `false` in `chat.tools.eligibleForAutoApproval` "always
  require[s] manual approval" and removes the auto-approval option from the confirmation dialog.
- AVAILABLE_UNTESTED — Sensitive-file edit approval is a related, separate control: "Configure
  sensitive-file approval when an edit must require confirmation before it is applied" (links to
  https://code.visualstudio.com/docs/agents/run/review-code-edits#_edit-sensitive-files).

---

## 6. Sandbox: prerequisites, filesystem/network controls, effective policy inspection

**Source:** "Sandbox Copilot agent sessions" — https://code.visualstudio.com/docs/agents/run/agent-sandboxing

### 6.1 Scope and boundary

- AVAILABLE_UNTESTED — Applies to "Copilot harness sessions that use the default tools in the shared
  runtime." "Sandboxing applies to terminal commands and their child processes. When configured,
  it also applies to MCP and language servers started or managed for the session. Built-in tools
  that don't start operating system processes use their own permission checks."
- AVAILABLE_UNTESTED — Important caveat: "Turning on sandboxing does not block outbound network access
  by default. `chat.agent.sandbox.network.allowNetwork` defaults to `true`."
- AVAILABLE_UNTESTED — "Sandboxing is not a virtual machine or user-account boundary, and it does not
  replace endpoint security. A sandboxed process still runs on the execution host under your
  account." "Sandboxing does not replace the isolation provided by cloud sessions or Dev
  Containers." Per-server MCP sandboxing is explicitly called out as "a separate feature with
  its own configuration and platform support" (see §7).

### 6.2 Platform prerequisites

| Platform | Prerequisite (as documented) |
|---|---|
| macOS | None. |
| Linux and WSL2 | Install `bubblewrap` and `socat` (e.g., `sudo apt-get install bubblewrap socat` on Debian/Ubuntu, `sudo dnf install bubblewrap socat` on Fedora). |
| Windows | Install "the applicable September 8, 2026 Windows security update" (version-specific). |

- AVAILABLE_UNTESTED — "WSL version 1 is not supported because it does not provide the Linux kernel
  features that `bubblewrap` requires."
- AVAILABLE_UNTESTED — For remote sessions: "install dependencies and operating system updates on the
  remote machine, not the machine where you connect to the session." The remote machine's
  platform determines the sandbox implementation.

### 6.3 Enabling and settings

- AVAILABLE_UNTESTED — `chat.agent.sandbox.enabled` controls sandboxing on all supported platforms;
  accepts `off`/`on`; **default `off`**.
- AVAILABLE_UNTESTED — "Sandbox device policies don't enforce these controls for Copilot harness
  sessions. Administrators should use Copilot managed settings to require sandboxing" (links to
  https://code.visualstudio.com/docs/enterprise/manage-ai-settings#_deploy-copilot-managed-sandbox-settings).
- AVAILABLE_UNTESTED — Full settings table:

  | Setting | Default | Purpose |
  |---|---|---|
  | `chat.agent.sandbox.enabled` | `off` | Enable sandboxing on all supported platforms |
  | `chat.agent.sandbox.network.allowNetwork` | `true` | Permit outbound network access, subject to hostname restrictions |
  | `chat.agent.sandbox.network.allowLocalNetwork` | `false` | Permit access to local-network hosts |
  | `chat.agent.sandbox.network.allowedDomains` | `[]` | Configure allowed network destinations |
  | `chat.agent.sandbox.network.deniedDomains` | `[]` | Configure blocked network destinations |
  | `chat.agent.sandbox.fileSystem.userConfiguredPaths` | empty | Customize read/write, read-only, denied paths |
  | `chat.agent.sandbox.fileSystem.allowDevToolAccess` | `true` | Grant access to common dev tools/config/caches |
  | `chat.agent.sandbox.mcpServers` | `true` | Sandbox locally launched MCP servers when sandboxing enabled |
  | `chat.agent.sandbox.lspServers` | `true` | Sandbox language servers when sandboxing enabled |
  | `chat.agent.sandbox.credentials.authenticategit` | `true` | Use user credentials for HTTPS Git ops inside sandbox |
  | `chat.agent.sandbox.credentials.authenticategh` | `true` | Use user's GitHub account for `gh` commands inside sandbox |
  | `chat.agent.sandbox.allowUnsandboxedCommands` | `true` | Permit requests to run outside the sandbox after confirmation |

- AVAILABLE_UNTESTED — "The sandbox automatically grants read and write access to the current working
  directory." `userConfiguredPaths` customizes read/write, read-only, and denied paths "on all
  supported platforms"; file-system and sandbox config in `.vscode/mcp.json`/settings "Only
  applies on macOS and Linux" per the related MCP configuration reference (see §7).
- AVAILABLE_UNTESTED — Session-level control: "Open Permissions and select Sandboxing for terminal to
  change sandboxing for the current session. This control does not update User or Workspace
  settings, change other sessions, or set the default for new sessions." A new session inherits
  the execution host's effective configuration.

### 6.4 Effective policy inspection

- AVAILABLE_UNTESTED — The Copilot harness documents **`/sandbox policy`** as a local inspection
  command to
  "verify whether sandboxing is active or investigate why a process is blocked." "This command
  isn't available in Local harness sessions." The report opens automatically as a Markdown
  preview (`sandbox-policy.md`) describing "the execution host, whether sandboxing is enabled,
  the operating system's sandbox implementation, and the effective file system and network
  policy." Works even when sandboxing is off ("the report indicates that sandbox restrictions
  are not active"). "The command does not start a model turn or change your sandbox settings."
  Re-run after changes to refresh; "To check the defaults for new sessions, start a new session
  before running the command." The command's documented inspectability is `AVAILABLE_UNTESTED`;
  this session did not run it, and its availability depends on confirming the Copilot harness.

---

## 7. MCP separate sandbox boundaries

**Sources:**
- "MCP configuration reference" — https://code.visualstudio.com/docs/agents/reference/mcp-configuration
- "Manage and troubleshoot MCP servers" — https://code.visualstudio.com/docs/agents/run/manage-mcp-servers
- "Understand trust and safety for AI agents" — https://code.visualstudio.com/docs/agents/concepts/trust-and-safety

- AVAILABLE_UNTESTED — Two config file formats exist with different capabilities (from the
  "Configuration file" capability comparison table): VS Code format (`.vscode/mcp.json` or user
  profile, top-level `servers`) supports "Development mode and per-server sandboxing" and
  file-system config such as working directory and environment file; the portable Copilot format
  (`.mcp.json` at workspace root, or `$COPILOT_HOME/mcp-config.json` / `~/.copilot/mcp-config.json`
  fallback, top-level `mcpServers`) does **not** list per-server sandboxing support in that
  comparison row. "The agent doesn't read `.vscode/mcp.json` directly" — VS Code forwards
  eligible servers to the Copilot harness session, excluding servers needing interactive input
  (`${input:...}`).
- AVAILABLE_UNTESTED — Sandbox configuration is a distinct top-level section in the VS Code MCP config
  format: `"sandbox": {}` — "an optional object that defines file system and network access
  rules for sandboxed servers... Only applies on macOS and Linux."
- AVAILABLE_UNTESTED — From "Manage and troubleshoot MCP servers" → "Sandbox MCP servers": "On macOS and
  Linux, per-server sandboxing restricts file system and network access for stdio MCP servers.
  Set `sandboxEnabled` in a supported VS Code-format configuration and define the required
  access rules." The same documentation states this mechanism is available on macOS and Linux;
  therefore per-server MCP sandboxing is **unavailable on the inspected Windows environment**
  according to the documented platform scope. This does not describe the distinct Copilot
  session-level sandbox for locally launched MCP servers.
- AVAILABLE_UNTESTED — From "Sandbox Copilot agent sessions": "it also applies to MCP and language
  servers started or managed for the session" (session-level sandbox, controlled by
  `chat.agent.sandbox.mcpServers`, default `true`) — this is the **session-level** sandbox
  boundary, explicitly distinguished from the **per-server** MCP sandbox: "Per-server MCP
  sandboxing is a separate feature with its own configuration and platform support."
- AVAILABLE_UNTESTED — MCP server trust is a separate boundary from sandboxing: "Workspace
  `.vscode/mcp.json` and `.mcp.json` configurations inherit Workspace Trust. Restricted mode
  prevents these servers from starting... Non-workspace sources can use a separate trust decision
  and might prompt on first start or after a configuration change." ("Review MCP configuration
  before trusting a repository because stdio servers can run code in their execution
  environment.")
- AVAILABLE_UNTESTED — Autostart behavior is independent of sandbox/trust: `chat.mcp.autostart`
  (`never`/`onlyNew`/`newAndOutdated`, default `newAndOutdated`) controls whether VS Code starts
  servers to discover tools on a new chat message; "Setting `chat.mcp.autostart` to `never`
  doesn't prevent the Copilot harness from starting its own configured MCP servers
  independently. To prevent a server from starting, disable it."

---

## 8. Subagents / delegation controls

**Source:** "Use subagents in Visual Studio Code" — https://code.visualstudio.com/docs/agents/run/subagents

- AVAILABLE_UNTESTED — Purpose: "A subagent works in its own context and returns a focused result to the
  main agent." Use cases given: research before implementation, comparing approaches
  (optionally with different models), and reviewing changes.
- AVAILABLE_UNTESTED — Subagent behavior is harness-specific ("Subagents by harness"):
  - **Copilot**: "manages delegation to built-in or custom subagents. Request a subagent in your
    prompt, or let the main agent decide when to delegate." For native config/behavior, refers to
    GitHub's own docs: https://docs.github.com/en/copilot/concepts/agents/copilot-cli/about-custom-agents.
  - **Claude**: "manages its own subagents, with separate context and configurable instructions
    and tools." Refers to https://code.claude.com/docs/en/sub-agents.
  - **Codex**: "uses provider-native subagents to run independent tasks and collect their
    results." Refers to https://developers.openai.com/codex/multi-agent.
  - **Local**: uses the VS Code **`runSubagent`** tool (also referenced as `agent/runSubagent` in
    the Configure Tools picker).
- AVAILABLE_UNTESTED — Local invocation mechanics: delegation is requested in natural language; "The
  main agent can also decide to delegate without an explicit request." "A Local subagent doesn't
  inherit the main conversation history" — the delegated task must be self-contained (goal,
  context, allowed actions, expected result). "Each Local invocation is stateless: the main agent
  can't send follow-up messages to the same subagent... The built-in tools for asking clarifying
  questions and managing todo items are unavailable to Local subagents."
- AVAILABLE_UNTESTED — Custom-agent-as-subagent configuration (cross-referenced from §2): `agents` field
  restricts a coordinator to named workers; must combine with `tools: ['agent']`;
  case-sensitive names; `disable-model-invocation: true` can be overridden by explicit listing;
  nested/self delegation requires `chat.subagents.allowInvocationsFromSubagents`.
- AVAILABLE_UNTESTED — Explicit note: "Provider documentation also covers CLI workflows. Commands and
  configuration options can differ in VS Code. Use the harness guide for integration-specific
  setup and limitations" — i.e., GitHub/Anthropic/OpenAI native-CLI subagent docs are not
  guaranteed to match VS Code behavior exactly.

---

## 9. Agent Skills

**Source:** "Use Agent Skills in VS Code" — https://code.visualstudio.com/docs/agent-customization/agent-skills

- AVAILABLE_UNTESTED — "Agent Skills are folders of instructions, scripts, and resources that AI agents
  can load when relevant to perform specialized tasks. Agent Skills is an open standard
  (agentskills.io) that works across multiple AI agents, including GitHub Copilot in VS Code,
  GitHub Copilot CLI, the GitHub Copilot app, Copilot cloud agent, and the experimental OpenAI
  Codex integration." "Unless otherwise noted, the configuration options and controls on this
  page apply to Copilot [harness]."
- AVAILABLE_UNTESTED — Explicit portability caveat: "Skills you create are portable across
  skills-compatible agents, but supported locations and optional features vary by agent."
- AVAILABLE_UNTESTED — Exact documented locations are project `.github/skills/`,
  `.claude/skills/`, and `.agents/skills/`; and personal `~/.copilot/skills/`,
  `~/.claude/skills/`, and `~/.agents/skills/`. For this Windows user, `~` represents the home
  directory. `chat.agentSkillsLocations` is a Local-only setting and is not read by Copilot.
- AVAILABLE_UNTESTED — Comparison vs. custom instructions (table): Skills teach "specialized
  capabilities and workflows" and can include "Instructions, scripts, examples, and resources,"
  are "Task-specific, loaded on-demand," and follow the open Agent Skills standard. Custom
  instructions define "coding standards and guidelines," are instructions-only, are "Always
  applied (or via glob patterns)," and use formats like `AGENTS.md` and
  `.github/copilot-instructions.md`.
- AVAILABLE_UNTESTED — For Local, `/skills` opens Configure Skills. For Copilot, use
  **Open Customizations** / the Agent Customizations editor.

---

## 10. Usage / telemetry surfaces

**Sources:**
- "Telemetry" — VS Code docs — https://code.visualstudio.com/docs/configure/telemetry
- "Viewing the Copilot usage metrics dashboard" — GitHub docs —
  https://docs.github.com/en/copilot/how-tos/administer-copilot/view-usage-and-adoption

### 10.1 VS Code editor telemetry

- AVAILABLE_UNTESTED — Three data types distinguished: **Crash Reports**, **Error Telemetry**, **Usage
  Data**. Controlled by a single setting, `telemetry.telemetryLevel`, with four levels (`all`,
  `error`, `crash`, `off`) each sending a different combination of the three data types (per the
  table on the page).
- AVAILABLE_UNTESTED — "To participate in the A/B experimentation and get early access to new features,
  you must have usage data enabled by setting `telemetry.telemetryLevel` to `all`." Disabling
  usage data (`error`/`crash`/`off`) disables experimentation-based feature rollout for that
  user, potentially delaying new-feature availability until GA.
- AVAILABLE_UNTESTED — "Extensions and telemetry": extensions "may be collecting their own usage data
  and are not controlled by the `telemetry.telemetryLevel` setting." Consult each extension's own
  documentation.
- AVAILABLE_UNTESTED — Local inspection surface: **Developer: Show Telemetry** command opens a
  **Telemetry** output channel and enables tracing; events are also logged to a local
  `telemetry.log` file, viewable via **Developer: Open Log...** → Telemetry.

### 10.2 GitHub Copilot usage metrics dashboard (org/enterprise-level)

- AVAILABLE_UNTESTED — Prerequisite: the **"Copilot usage metrics"** policy must be enabled at the
  enterprise or organization level (links to
  https://docs.github.com/en/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-enterprise-policies#defining-policies-for-your-enterprise
  and
  https://docs.github.com/en/copilot/how-tos/administer-copilot/manage-for-organization/manage-policies#enabling-copilot-features-and-models-in-your-organization).
- AVAILABLE_UNTESTED — Access path: Enterprise → **Insights** tab → **Copilot usage** (left sidebar).
- AVAILABLE_UNTESTED — "Data in the dashboard is primarily based on IDE telemetry and is supplemented by
  server-side telemetry to capture additional active users. Data may appear up to three full UTC
  days behind the current date."
- AVAILABLE_UNTESTED — NDJSON export supported for deeper analysis via Copilot Chat prompts (example
  queries given for interaction-count vs. acceptance-activity analysis).
- Note: this dashboard is an **org/enterprise-administration surface**, distinct from any
  personal "my usage" view; this research did not locate (and did not need, per the no-local/
  no-enterprise-system-access constraint) a personal usage page beyond the VS Code editor
  telemetry controls in §10.1.

### 10.3 OpenTelemetry agent-interaction monitoring (verification of a telemetry statement against first-party docs)

**Scope note:** this subsection verifies specific claims about Copilot Chat's agent-level OTel
surface — traces, metrics, events, tool executions, requested/resolved models, token-usage/cache
fields, parent/subagent trace connectivity, and enable/config/export controls — against two
first-party pages. It does not change the inventory-only, read-only nature of P0A (see the
document header and §0): nothing below was enabled, configured, exported, or demonstrated in this
or any session, and current-environment availability/entitlement/approval remain unresolved
(`REQUIRES_PROTOTYPE`), consistent with the rest of this document.

**Sources:**
- "Monitor agent usage with OpenTelemetry" — VS Code docs —
  https://code.visualstudio.com/docs/agents/guides/monitoring-agents (canonical path; also
  reachable at the legacy `/docs/copilot/guides/monitoring-agents` URL, which redirects to it).
  Headings cited: "What gets collected" / "Attribute namespaces" / "Traces" / `invoke_agent` span /
  `chat` span / `execute_tool` span / `execute_hook` span / "Metrics" / "Events" / "Resource
  attributes" / "Identity capture" / "Content capture" / "Enable OTel monitoring" / "VS Code
  settings" / "Environment variables" / "Commands" / "Manage OTel configuration for your
  organization" / "Trace structure by harness" / "Security and privacy."
- "Manage telemetry in enterprise environments" — VS Code docs —
  https://code.visualstudio.com/docs/enterprise/telemetry. Heading cited: "OpenTelemetry support
  for agent interactions."
- "Enterprise managed settings" — GitHub Copilot docs —
  https://docs.github.com/en/copilot/reference/enterprise-administrators/enterprise-managed-settings#telemetry
  (referenced from the VS Code page's "Manage OTel configuration for your organization" section;
  fetched to confirm a `telemetry` managed-settings key exists and lists per-client support, but
  its field-by-field detail was not transcribed here).

**Verified/corrected claims:**

- AVAILABLE_UNTESTED — Confirmed: "Copilot Chat can export traces, metrics, and events via
  OpenTelemetry (OTel)... giving you visibility into agent interactions, LLM calls, tool
  executions, and token usage." All three signal kinds (traces, metrics, events) are documented,
  not just traces/metrics as a narrower statement might claim.
- AVAILABLE_UNTESTED — Confirmed, with a precision correction: signal names/attributes follow the
  **OTel GenAI Semantic Conventions** (`gen_ai.*`), but Copilot Chat also emits two Copilot-specific
  namespaces — `github.copilot.*` (preferred for new dashboards) and a **legacy** `copilot_chat.*`
  namespace (original VS Code extension namespace, "continue to emit indefinitely," "no sunset
  date"). A statement asserting only `gen_ai.*` attributes, or asserting `copilot_chat.*` is
  deprecated/removed, would be inaccurate.
- AVAILABLE_UNTESTED — Confirmed: **requested vs. resolved model** are both captured as distinct
  attributes — `gen_ai.request.model` ("Requested model") and `gen_ai.response.model` ("Resolved
  model") — on both the `invoke_agent` span and the `chat` span. This supports a claim that the
  product distinguishes requested-model-from-resolved-model, but only as documented span
  attributes, not as a demonstrated local capture.
- AVAILABLE_UNTESTED — Confirmed, with field-level precision: **token usage** fields are
  `gen_ai.usage.input_tokens` and `gen_ai.usage.output_tokens` (per-session total on
  `invoke_agent`, per-call on `chat`). **Cache token fields exist but are two distinct fields, not
  one**: `gen_ai.usage.cache_read.input_tokens` ("Cache-read input tokens, when available") and
  `gen_ai.usage.cache_creation.input_tokens` ("Cache-creation input tokens, when available"). A
  statement referring to a single generic "cache tokens" field without distinguishing cache-read
  from cache-creation undercounts the documented schema. A reasoning-token field also exists:
  `gen_ai.usage.reasoning.output_tokens` (with a legacy alias `gen_ai.usage.reasoning_tokens`).
  All four token-count attributes are explicitly scoped "when available" / provider-dependent,
  not guaranteed per call.
- AVAILABLE_UNTESTED — Confirmed: **tool executions** are captured as one `execute_tool` span per
  tool invocation, with `gen_ai.tool.name`, `gen_ai.tool.type` (`function` or `extension` for MCP
  tools), `gen_ai.tool.call.id`, and several `github.copilot.tool.parameters.*` fields (edit type,
  skill name, MCP server-name hash/tool name, file path). Tool call **arguments and results**
  (`gen_ai.tool.call.arguments`, `gen_ai.tool.call.result`) are explicitly marked **content-capture
  only** (off by default) — a statement implying tool arguments/results are always captured would
  be incorrect.
- AVAILABLE_UNTESTED — Confirmed: **parent/subagent trace connectivity** is documented, not merely
  assumed. "When an agent invokes a subagent (for example, through the `runSubagent` tool), the
  trace context is automatically propagated. The subagent's `invoke_agent` span appears as a child
  of the parent agent's `execute_tool` span, producing a connected trace tree across async
  boundaries." This connectivity is harness-dependent, however: under "Trace structure by harness,"
  Copilot CLI terminal sessions are explicitly **not** linked — "Terminal traces appear as
  independent root traces under service `github-copilot` and are not linked to extension traces" —
  and for the Claude agent integration, subagent calls nest under a Claude-specific `Agent` tool
  span rather than the generic `runSubagent` path. A blanket claim that "all subagent traces
  connect to the parent trace" would need this per-harness qualification.
- AVAILABLE_UNTESTED — Confirmed: **OTel enable controls** are OR'd, not single-switch: OTel
  activates when *any* of `github.copilot.chat.otel.enabled=true`,
  `github.copilot.chat.otel.dbSpanExporter.enabled=true`, `COPILOT_OTEL_ENABLED=true`, or
  `OTEL_EXPORTER_OTLP_ENDPOINT` being set is true. All are documented as `false`/unset by default,
  and the page states OTel is "off by default and emits no data until you explicitly enable it,"
  with "the OTel SDK is not loaded when disabled, resulting in zero runtime overhead." No OTel
  control was enabled in this session.
- AVAILABLE_UNTESTED — Confirmed: **export/config controls** include an exporter-type setting
  (`github.copilot.chat.otel.exporterType`: `otlp-http` default, or `otlp-grpc`/`console`/`file`),
  an endpoint setting/env-var pair, content-capture and identity-capture toggles (both default
  `false`, each independently gated, and identity capture is explicitly scoped: "Identity capture
  currently applies only to Local harness sessions. Telemetry from the Agent Host process doesn't
  include these identity attributes"), a max-attribute-size setting, a file-output path, and a
  local SQLite span-export path (`dbSpanExporter.enabled` + **Chat: Export Agent Traces DB**
  command). Environment variables take precedence over VS Code settings, except that
  organization-mandated ("Copilot managed settings") values override user settings, and in turn
  OTel environment variables can still override managed values in the Copilot Chat extension — a
  three-tier precedence chain, not a flat override order.
- AVAILABLE_UNTESTED — **Availability vs. entitlement/approval, clarified as two distinct
  questions:** (1) *Product-documented availability* — the OTel settings, span/metric/event
  schema, and exporter options above are documented as generally available in the cited VS Code
  docs page (no edition/plan gate is stated for the settings themselves). (2) *Organizational
  entitlement/approval* — a **separate, enterprise-administration-scoped** control plane exists:
  "Enterprises can mandate OTel export configuration centrally through Copilot enterprise-managed
  settings, so that telemetry flows to an approved collector without each developer setting
  `OTEL_*` environment variables," documented in GitHub's "Enterprise managed settings" reference
  (`telemetry` key, confirmed present with per-client support columns including VS Code) and in
  VS Code's "Apply managed telemetry in VS Code" section of
  https://code.visualstudio.com/docs/enterprise/manage-ai-settings. Neither this subsection nor
  any other part of P0A establishes whether such managed settings exist, are enforced, or are
  approved for the current organization/environment — that remains `REQUIRES_PROTOTYPE` /
  out of scope for a read-only, non-enterprise-contacting inventory.
- AVAILABLE_UNTESTED — Minor but citation-relevant: the general VS Code **editor** telemetry policy
  (`TelemetryLevel`, §10.1) and the Copilot Chat **agent-interaction OTel** surface (this
  subsection) are governed by different controls and different docs pages. A statement that
  disabling `telemetry.telemetryLevel` also disables/enables agent OTel traces, or vice versa,
  would conflate two independently documented systems; the "Manage telemetry in enterprise
  environments" page lists them as separate sections ("Telemetry-related policies" vs.
  "OpenTelemetry support for agent interactions").

**P0F placement consistency (plan/ADR cross-check):**

- CONSISTENT — `Generic_Supervised_Agent_Graph_Implementation_Plan.md` lists `P0F: Telemetry and
  context-sizing feasibility` as a distinct pre-specification milestone, separate from `P0A:
  Environment and harness inventory` (same file, "Pre-Specification Product-Enforcement Gate"
  list). Placing telemetry **prototype/testing and approval** work in P0F — rather than folding it
  into P0A's read-only inventory — matches this milestone split, and matches the plan's explicit
  statement that "Telemetry export requires organizational approval of fields, storage, access,
  and retention" (same file, "Telemetry" section) and that specialist work requires local
  audit/policy/sandbox tests first while "optional provider token, model, or cost telemetry is not
  a prerequisite for read-only fixture work" — i.e., telemetry proof is explicitly deferred past
  the audit-first gate, not bundled into the environment-inventory step.
- CONSISTENT — [ADR 0009](../adr/0009-provider-usage-telemetry.md) ("Provider Usage Telemetry")
  requires that "telemetry export requires organizational approval of fields, storage, access, and
  retention," which is an approval/governance step, not an inventory step — this belongs with
  P0F's feasibility-and-prototype work, not P0A's documentation-only pass.
- CONSISTENT — [ADR 0010](../adr/0010-audit-before-usage-telemetry.md) ("Audit Before Usage
  Telemetry") explicitly defers "provider-usage telemetry proof to M3A" and keeps provider
  token/model/cost telemetry optional and non-blocking for earlier specialist work — reinforcing
  that telemetry prototyping/testing is a later, separately-gated activity rather than part of the
  P0A environment-inventory deliverable this file represents.
- Caveat: this cross-check compares the OTel documentation findings above against the plan/ADR
  text as currently committed in this repository; it does not confirm P0F has been executed, does
  not confirm organizational approval has been sought or granted, and does not confirm the current
  session's environment has telemetry export entitlement. Those remain open, later-milestone
  questions.

---

## 11. Org restrictions (GitHub Copilot enterprise/organization policies)

**Sources:**
- "GitHub Copilot policies for enterprises and organizations" — https://docs.github.com/en/copilot/concepts/enterprise/policies
- "Managing policies and features for GitHub Copilot in your organization" — https://docs.github.com/en/copilot/how-tos/administer-copilot/manage-for-organization/manage-policies
- "Enterprise managed settings" — https://docs.github.com/en/copilot/reference/enterprise-administrators/enterprise-managed-settings
- "Manage AI settings in enterprise environments" — VS Code docs — https://code.visualstudio.com/docs/enterprise/manage-ai-settings

### 11.1 Three distinct management layers (VS Code framing)

- AVAILABLE_UNTESTED — From "Manage AI settings in enterprise environments": organizations govern AI
  features in VS Code through **three** solutions that should not overlap for the same control:
  1. **Copilot enterprise-managed settings** — "guardrails that should apply across supported
     clients, such as VS Code and Copilot CLI," delivered via "server-managed, MDM-managed, or
     file-based delivery methods," using "the Copilot configuration format and delivery path, not
     the VS Code device-policy namespace" even when delivered via MDM. Administrators can
     override settings per enterprise team (server-managed deployments).
  2. **VS Code device policies** — "editor-specific controls on managed VS Code installations,"
     deployed via platform tools (Group Policy, Intune, macOS configuration profiles); "apply to
     the managed device and override the corresponding user settings."
  3. **GitHub organization and enterprise settings** — "account-level and service-side controls,
     such as Copilot access, model availability, content exclusions, and organization-provided
     customizations," following "the signed-in GitHub account and its organization or enterprise
     membership rather than the VS Code device-policy channel."
- AVAILABLE_UNTESTED — Guidance table maps each requirement type to the correct solution (e.g.,
  "Apply consistent Copilot guardrails across supported clients" → enterprise-managed settings;
  "Control a VS Code-specific editor feature on managed devices" → VS Code device policies;
  "Control Copilot access or GitHub-hosted org/enterprise behavior" → GitHub org/enterprise
  settings). "Some controls are available through more than one solution. Review the order of
  precedence before you combine them."

### 11.2 GitHub-side policy mechanics

- AVAILABLE_UNTESTED — "Policies control which GitHub Copilot features, agents, and models your users
  can access, and how they can use those features." Set first at enterprise level; "For most
  policies, enterprise administrators can either explicitly enable or disable a policy, or let
  organizations decide." Exception: for **Copilot cloud agent**, "enterprises can select exactly
  which organizations receive access."
- AVAILABLE_UNTESTED — Scope: "Generally, policies only apply to users on your Copilot plan... A small
  number of policies work differently and govern a setting for everyone," e.g., "you can block
  Copilot cloud agent for all users in your enterprise's repositories."
- AVAILABLE_UNTESTED — Conflict resolution for users with multiple licenses: "If these organizations have
  configured the same policy differently, the **least restrictive** policy usually applies, but
  there are some exceptions." Across different enterprises: "the **most restrictive** policy
  across enterprises almost always applies" (example given: disabling Copilot Chat in GitHub at
  any enterprise disables it for the user everywhere).
  Full conflict-resolution detail is deferred to a separate reference page,
  "Feature availability when GitHub Copilot policies conflict in organizations" —
  https://docs.github.com/en/copilot/reference/enterprise-administrators/policy-conflicts
  (linked, not independently fetched in this research pass).
- AVAILABLE_UNTESTED — Default status: "two separate policies control whether unconfigured generally
  available (GA) features and models default to enabled or disabled" ("The models policy is
  already active. The feature policy will become active soon" — per an in-page time-boxed note
  marked to expire 2026-10-22, i.e., very close to this research date; **treat this specific
  claim as time-sensitive and re-verify if used after that date**).
- AVAILABLE_UNTESTED — Organization-level configuration path (from "Managing policies and features..."):
  org Settings → **Policies** tab (privacy/feature availability for licensed users) and
  **Models** tab (model availability, some with added cost). Enterprise-level policy settings
  can override/constrain org-level choices per §11.2's conflict rules above.

### 11.3 Enterprise managed settings (schema)

- AVAILABLE_UNTESTED — "Enterprise managed settings" reference describes a `managed-settings.json`
  schema with a **Supported keys** table whose columns separately enumerate coverage for
  **Copilot CLI**, **VS Code**, **the GitHub Copilot app**, **Copilot cloud agent**, and
  **JetBrains IDEs** — i.e., individual managed-setting keys can apply to a subset of these
  clients, not uniformly to all. (The full keys/values table was not completely retrieved in this
  pass due to content truncation; only the column structure/client coverage pattern is confirmed
  here. A follow-up fetch of the full table would be needed to cite specific key names relevant
  to P0A, e.g., any keys governing custom agents, hooks, sandboxing, or MCP servers specifically.)
- Cross-reference: VS Code's own approvals page already cites two enterprise-managed-settings
  anchors directly relevant to P0A — permission-level management
  (https://code.visualstudio.com/docs/enterprise/manage-ai-settings#_manage-permission-levels)
  and mandatory sandboxing deployment
  (https://code.visualstudio.com/docs/enterprise/manage-ai-settings#_deploy-copilot-managed-sandbox-settings)
  — confirming that at least permission levels and sandbox enforcement are governable via this
  channel, per §5 and §6.3 above.

---

## 12. Gaps / follow-ups identified during this research pass

These are explicitly flagged rather than silently omitted, per the evidence-based inventory
requirement:

1. **VS Code Copilot hook coverage:** the Copilot CLI reference lists events and payloads, but
   the VS Code hooks page says to verify event availability for the selected VS Code version.
   The active harness/version is unresolved; do not treat the CLI event list as certified VS
   Code coverage.
2. **Enterprise managed settings full key table** (`managed-settings.json` schema) was only
   partially retrieved; specific key names relevant to custom agents, hooks, sandboxing, and MCP
   were not individually confirmed beyond the cross-references noted in §11.3.
3. **Effective organization policy** was not inspected, and no locally applied managed-settings
   payload was available in the read-only evidence gathered for this session.
4. **Windows prerequisite versions** for Copilot terminal sandboxing and PowerShell hook examples
   were not checked. The documentation's Windows update and PowerShell 7 requirements are recorded
   above, but local installation is unknown.
5. A **time-sensitive claim** about default GA feature/model policy activation (marked to expire
   2026-10-22 in GitHub's own docs) was captured verbatim in §11.2 and should be re-verified if
   this document is consulted after that date, since the underlying docs content is explicitly
   scheduled to change.
6. No distinct **personal/individual** Copilot usage-metrics surface (as opposed to the
   org/enterprise administration dashboard in §10.2) was located during this pass; if P0A
   requires a personal usage view, that is an open gap.

---

## Citation index (quick reference)

| Topic | URL |
|---|---|
| Agent harnesses overview | https://code.visualstudio.com/docs/agents/run/agent-harnesses |
| Custom agents (create/manage) | https://code.visualstudio.com/docs/agent-customization/custom-agents |
| Custom agent file reference | https://code.visualstudio.com/docs/agents/reference/custom-agent-file-reference |
| Tools with agents | https://code.visualstudio.com/docs/agents/run/tools |
| Hooks (Preview) | https://code.visualstudio.com/docs/agent-customization/hooks |
| VS Code Local hooks reference | https://code.visualstudio.com/docs/agents/reference/hooks-reference |
| GitHub Copilot hooks overview | https://docs.github.com/en/copilot/concepts/agents/hooks |
| GitHub Copilot hooks reference | https://docs.github.com/en/copilot/reference/hooks-reference |
| GitHub Copilot CLI hook setup | https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/use-hooks |
| GitHub Copilot cloud agent hook setup | https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/use-hooks |
| Approvals and permissions | https://code.visualstudio.com/docs/agents/run/approvals |
| Trust and safety concepts | https://code.visualstudio.com/docs/agents/concepts/trust-and-safety |
| Agent sandboxing | https://code.visualstudio.com/docs/agents/run/agent-sandboxing |
| MCP configuration reference | https://code.visualstudio.com/docs/agents/reference/mcp-configuration |
| Manage/troubleshoot MCP servers | https://code.visualstudio.com/docs/agents/run/manage-mcp-servers |
| Subagents | https://code.visualstudio.com/docs/agents/run/subagents |
| Agent Skills | https://code.visualstudio.com/docs/agent-customization/agent-skills |
| VS Code telemetry | https://code.visualstudio.com/docs/configure/telemetry |
| Monitor agent usage with OpenTelemetry | https://code.visualstudio.com/docs/agents/guides/monitoring-agents |
| Manage telemetry in enterprise environments (VS Code) | https://code.visualstudio.com/docs/enterprise/telemetry |
| Copilot usage metrics dashboard | https://docs.github.com/en/copilot/how-tos/administer-copilot/view-usage-and-adoption |
| Copilot policies (enterprise/org concepts) | https://docs.github.com/en/copilot/concepts/enterprise/policies |
| Managing org policies | https://docs.github.com/en/copilot/how-tos/administer-copilot/manage-for-organization/manage-policies |
| Enterprise managed settings reference | https://docs.github.com/en/copilot/reference/enterprise-administrators/enterprise-managed-settings |
| Manage AI settings in enterprise environments (VS Code) | https://code.visualstudio.com/docs/enterprise/manage-ai-settings |

---

*End of P0A deliverable: `docs/compatibility/selected-environment.md`. No local commands were
executed by the documentation research subagent; the parent-session read-only commands and
observations are recorded in §0. No authentication/configuration was performed, and no
hooks/agents were created. Product claims are sourced from publicly fetched, first-party
documentation pages cited inline.*
