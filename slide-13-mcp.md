# Chapter 13: MCP — Connect Claude Code to Every Tool in Your Stack

## Model Context Protocol: Open Standard by Anthropic. 3,000+ Community Servers.

---

The Model Context Protocol (MCP) is how Claude Code reaches beyond your codebase into the rest of your engineering stack. It is an **open standard** created by Anthropic, and it has already grown to over 3,000 community-built servers.

### What MCP Does

MCP is a standardized way for Claude Code to connect to external tools, databases, and APIs. It functions as a universal connector — analogous to USB for AI agents — that works the same way regardless of what is being plugged in.

When a Claude Code session starts, MCP servers are auto-discovered and their tool definitions are loaded. Claude then invokes these tools based on context — if a JIRA ticket is mentioned, Claude uses the JIRA MCP server to fetch it. If a Slack conversation is referenced, Claude reads it via the Slack server.

**Tool Search** is an optimization: instead of loading all tool schemas upfront (which consumes context), Claude can discover tools on-demand. This keeps the context window lean while providing access to thousands of potential tools.

### Available Integrations

The ecosystem is vast:

- **GitHub** — PRs, issues, code search, actions
- **Databases** — SQL, Postgres, SQLite, MongoDB
- **Slack** — messages, channels, threads
- **Browser** — Playwright for DOM interaction and screenshots
- **JIRA** — tickets, sprints, boards
- **Figma** — design files, components
- **Sentry** — error tracking, releases
- **Notion** — documents, databases
- And 3,000+ more community servers

### How to Configure

Adding an MCP server is one command:

```bash
claude mcp add github -e GITHUB_TOKEN \
  -- npx -y @modelcontextprotocol/server-github
```

This registers the GitHub server, passes your token as an environment variable, and starts it using npx.

**Scoping:**
- `--scope project` — config saved in `.mcp.json` in your repo (shared with team via git)
- `--scope user` — config saved in your user profile (personal, applies everywhere)

For team adoption, always use project scope and commit `.mcp.json` to your repository. This way, every developer who clones the repo gets the same MCP integrations automatically.

### Key Capabilities

**Elicitation** — MCP servers can request structured input mid-task. For example, a database server might ask "Which schema do you want to query?" and present options in an interactive dialog.

**OAuth** — Browser-based authentication. When Claude needs access to a service, it opens the browser for the OAuth flow. Authentication happens once, and the token is stored for future sessions.

**Resources** — MCP resources can be `@mentioned` in prompts just like local files. For example, `@jira:PROJ-1234` to reference a JIRA ticket or `@slack:#engineering` to reference a channel.

**Permissions** — Every MCP tool call requires explicit approval by default. Auto-approve can be configured for trusted servers in settings, but the default is secure.

### Enterprise Configuration Example

Below is a `.mcp.json` for a typical enterprise team:

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": { "GITHUB_TOKEN": "${GITHUB_TOKEN}" }
    },
    "postgres": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres"],
      "env": { "DATABASE_URL": "${DATABASE_URL}" }
    },
    "slack": {
      "command": "npx",
      "args": ["-y", "@anthropic/mcp-server-slack"],
      "env": { "SLACK_TOKEN": "${SLACK_TOKEN}" }
    }
  }
}
```

Check this into the repo. Every developer on the team gets GitHub, Postgres, and Slack integration from day one. Environment variables are resolved from each developer's local environment, so no secrets are committed.

### Transport Options

MCP supports three transport protocols:
- **stdio** — local process communication (most common, used above)
- **SSE** — Server-Sent Events for remote servers
- **HTTP** — standard HTTP for cloud-hosted MCP servers

---

## Real-World Workflow Example

```
You: Read JIRA-4567, check the linked GitHub PR,
     run the failing test mentioned in the ticket,
     and post a summary to #eng-bugs on Slack.

Claude Code:
  1. [MCP:JIRA] Fetches JIRA-4567 — "Search pagination returns duplicates"
  2. [MCP:GitHub] Reads linked PR #892 — sees the attempted fix
  3. [Tools:Bash] Runs the failing test — confirms it still fails
  4. [Reasoning] Identifies the root cause the PR missed
  5. [MCP:Slack] Posts summary to #eng-bugs:
     "JIRA-4567: PR #892 doesn't fix the root cause.
      The issue is in the OFFSET calculation, not the WHERE clause.
      I can submit a corrected PR if approved."
```

Four different systems, one coherent workflow, zero context switching.
