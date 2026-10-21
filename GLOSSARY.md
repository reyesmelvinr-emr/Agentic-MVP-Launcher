# Supervised Agent Graph

Vocabulary for the reusable supervised-agent framework and its project profiles.

## Language

**Framework core**:
Project-independent contracts and components shared by projects created from the framework.

**Project profile**:
Project-specific configuration of objectives, risk, integrations, context, approvals, targets, and acceptance criteria.

**Project extension**:
A project-specific agent or skill added for a distinct workflow or capability without changing the framework core.

**Risk profile**:
A project classification that selects additional safeguards without weakening the framework's minimum controls.

**Framework version**:
A pinned release of the reusable core and its contracts against which a project is maintained and upgraded.

**Agent**:
A configured task-performing role with a defined purpose, instructions, tools, and applicable capability, identity, and sandbox profiles.

**Specialist**:
An agent assigned a bounded task in a particular domain.

**Subagent**:
An agent invocation delegated by a parent run with bounded context; it does not inherit approval or authority from its parent.

**Agent Skill**:
A reusable task procedure that an agent can apply; it is not an independent agent identity or permission.

**Instruction file**:
Persistent guidance applied to a scope of work; it does not enforce permissions by itself.

**Hook**:
A harness lifecycle callback whose enforcement scope is limited to events and tools verified to pass through it.

**Script**:
A deterministic executable used to perform repeatable operations; it does not grant authority by itself.

**MCP tool**:
A tool capability exposed through Model Context Protocol, with an execution and access boundary that must be assessed independently.

**MCP boundary**:
The independent execution, identity, credential, network, and logging scope of an MCP server and its tools.

**Execution identity**:
The actual authenticated principal and permissions available to an agent's execution; a declared profile does not reduce them.

**Execution manifest**:
The immutable, versioned description of the exact tools, actions, paths, targets, and constraints proposed for an execution.

**Approval**:
An operator's time-limited authorization for an exact, reviewed execution scope; it does not extend to changed scope or another run.

**Pipeline**:
A separately governed CI/CD execution environment with its own identity, permissions, and approval controls.

**Coordinator**:
A low-privilege role that assembles bounded work, recommends specialists and model tiers, and requests state transitions without implementing work, approving actions, or directly advancing project state.

**Task packet**:
A version-bound assignment that defines a task's objective, scope, allowed context and actions, outputs, and completion criteria. It does not replace required approvals.

**Authoritative source**:
A designated record that governs a specific subject; other sources defer to it when they conflict.

**Advisory summary**:
A derived account of source material that links to evidence but does not supersede its authoritative sources.

**Untrusted context**:
Repository, external, or log content supplied as data rather than as instructions or authority.

**Context budget**:
A limit on the source material assembled for an individual task to keep its working context bounded.

**Progress criterion**:
A pre-defined, evidence-backed condition used to determine whether a milestone deliverable is complete.

**Capability profile**:
The declared scope of tools and actions intended for an agent. It does not grant or reduce permissions held by the execution identity.

**Identity profile**:
A description of the identity and external permissions available to an agent's execution; it is distinct from the agent's declared capabilities.

**Sandbox**:
An execution boundary that restricts supported process access to resources such as files, network, and child processes. It is not a substitute for external authorization or identity controls.

**State event**:
An append-only record of an authorized project-state transition. It records a transition and does not authorize or replay associated external actions.

**State Engine**:
The sole authority for validating and recording project-state transitions and maintaining the current state projection.

**State projection**:
The current project-state view derived from state events and rebuildable from valid event history.

**Needs review**:
A run status requiring operator investigation before resumption or closure because execution may have produced an uncertain partial effect or invalid state history.

**Blocked**:
A run status indicating that a prerequisite or control prevents the task from starting or continuing.

**Failed**:
A run status indicating the task did not complete and its effects are known.

**Rolled back**:
A run status indicating a rollback was performed and its outcome was verified.

**Tier recommendation**:
A suggested model-routing tier for a task, not a guarantee of the model actually selected or a spending limit.

**Worktree**:
An isolated Git working directory for source changes. It does not isolate process access or provide a security boundary.

**Handoff**:
A durable record of run status, results, evidence, and unresolved items passed between sessions or agents. It does not grant approval or transfer authorization.

**Test seam**:
A stable observable interface through which behavior can be stimulated and its result assessed.

**Independent oracle**:
A reviewed source of expected results independent of the production behavior being evaluated.

**Verification evidence**:
A record supporting a claim about observed behavior or a control, including its scope and limitations.

**Behavioral evidence**:
Evidence of how an agent or operator behaved; it does not by itself prove that a prohibited action is technically blocked.

**Enforcement evidence**:
Evidence that a supported technical control blocks an action within the tested scope; it does not establish protection outside that scope.