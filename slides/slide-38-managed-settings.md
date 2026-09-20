# Slide 38: Managed settings

### Governance starts with defaults and enforced policy

- Managed settings let an organization establish controls that local project or user configuration cannot override when a boundary must apply everywhere.
- Use them for centrally managed permissions, restricted modes, approved MCP servers, network or sandbox policy, authentication requirements, and cost or observability controls.

| Governance goal | Managed control example |
| --- | --- |
| Prevent unsafe modes | Disable Auto or bypass mode where policy requires it |
| Restrict external capability | Allow only approved MCP servers and tool scopes |
| Enforce critical boundaries | Central `deny` rules for protected paths or commands |
| Standardize reporting | Managed telemetry and contracted-rate cost reporting |

### Deploy policy like production configuration

- Document the purpose, owner, rollout, exception path, and test case for every managed control.
- Test policy from a normal developer session, not only an administrator account. Confirm effective settings with `/status`, `/permissions`, and the relevant integration panel.
- Defaults reduce drift; they do not remove the need for plan approval, review, or accountable engineers.

### Example

An organization disables bypass mode, denies unapproved MCP servers, requires an `ask` prompt for production deploy commands, and verifies from a standard developer workstation that the policy cannot be bypassed locally.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Context window](https://code.claude.com/docs/en/context-window)
- [Prompt caching](https://code.claude.com/docs/en/prompt-caching)
- [Claude Code costs](https://code.claude.com/docs/en/costs)
- [Managed settings](https://code.claude.com/docs/en/managed-settings)

[← Previous slide](slide-37-in-practice.md) · [Deck index](../README.md)