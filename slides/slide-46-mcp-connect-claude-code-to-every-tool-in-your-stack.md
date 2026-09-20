# Slide 46: MCP: connect Claude Code to every tool in your stack

### MCP is a controlled bridge to external tools

- MCP servers expose external resources and actions, such as GitHub issues, monitoring data, databases, design systems, and internal APIs, to Claude Code.
- Each server is a privileged integration. Review what it can read, write, authenticate to, and send outside the repository before enabling it.

### Minimum integration review

| Question | Good control |
| --- | --- |
| Does the workflow truly need external access? | Prefer local files, a CLI, or a narrow API when sufficient |
| Which data can it read or send? | Least-privilege credentials and scoped OAuth permissions |
| Which actions can it take? | Allow/ask/deny rules for specific MCP tools |
| Who can enable it? | Project review, workspace trust, or managed MCP policy |
| Does it need to load every session? | Disable unused servers; rely on tool search for on-demand discovery |

### Example: GitHub context without release access

Enable an approved GitHub integration for issue and pull-request context. Keep deployment and infrastructure MCP servers disabled for this repository, require approval for write-capable GitHub actions, and review the server configuration before it is shared in `.mcp.json`.

MCP extends capability; it does not eliminate the need for permissions, trust review, or human approval.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [MCP integration](https://code.claude.com/docs/en/mcp)
- [MCP specification](https://modelcontextprotocol.io/specification/2025-06-18)

[← Previous slide](slide-45-subagents-isolate-specialize-and-parallelize-work.md) · [Deck index](../README.md)