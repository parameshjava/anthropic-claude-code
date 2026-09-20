# Slide 47: Enterprise deployment and governance

### Enterprise rollout is more than access; it is policy and controls

Enterprise deployment should make the safe, observable path the easiest path for every developer.

| Control plane | Questions to settle before rollout |
| --- | --- |
| **Identity** | Who can use the tool? How do SSO, provisioning, and offboarding work? |
| **Policy** | Which modes, tools, MCP servers, directories, and commands are allowed or blocked? |
| **Data** | Which provider, region, retention, and data-handling commitments apply to this deployment? |
| **Spend** | Where are budgets, usage limits, and model defaults managed? |
| **Observability** | Which usage, audit, and telemetry data is collected, and who can access it? |
| **Ownership** | Who owns policy changes, exceptions, incident response, and periodic review? |

### Roll out in stages

1. Choose a pilot repository and define its allowed tasks, prohibited actions, and verification bar.
2. Deploy managed defaults and reviewed project guidance; test the effective policy from a normal developer session.
3. Enable only approved integrations and least-privilege credentials.
4. Measure quality, cost, friction, and incidents; refine policy before expanding access.

### Example

Before organization-wide access, define SSO and provisioning, managed permission policy, approved MCP servers, spend reporting, telemetry retention, release controls, and the engineering owner for every exception path.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Administration](https://code.claude.com/docs/en/admin-setup)
- [Managed settings](https://code.claude.com/docs/en/managed-settings)

[← Previous slide](slide-46-mcp-connect-claude-code-to-every-tool-in-your-stack.md) · [Deck index](../README.md)