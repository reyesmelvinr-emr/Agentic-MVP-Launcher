# One-Time Execution Approval

An approval authorizes one exact execution scope and is bound to the task packet and manifest, expected state version, operator identity, repository/branch, paths, tools/actions, targets/domains, and expiry. Scope changes require new approval; expired, revoked, consumed, or mismatched approvals are rejected, and approval is never transferred by a handoff or subagent result.
