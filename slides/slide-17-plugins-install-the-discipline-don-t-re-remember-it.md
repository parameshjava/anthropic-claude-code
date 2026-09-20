# Slide 17: Plugins: install the discipline, don't re-remember it

### Ten high-value plugins for common engineering workflows

This is a curated starting set from Anthropic's official marketplace, selected for common software-engineering work rather than a popularity ranking. A plugin may provide commands, automatic skills, agents, hooks, code intelligence, or MCP tools.

Before installing any plugin, inspect its **Discover** details pane for commands, skills, agents, hooks, MCP/LSP servers, context cost, scope, and source. Use `/help` afterward to see what is active in the current version.

### 1. Commit Commands

**Use it for:** repository-style commits, creating a branch and pull request, and removing local branches deleted from the remote.

**Install**

```text
/plugin install commit-commands@claude-plugins-official
```

**Commands:**

- `/commit`: review staged and unstaged changes, generate a repository-style message, stage relevant files, and create one commit. Use after a small completed, tested unit of work.
- `/commit-push-pr`: create a feature branch when needed, commit, push, and open a GitHub pull request. Use only after reviewing the diff and test evidence; it requires authenticated `gh`.
- `/clean_gone`: remove local branches and related worktrees already deleted from the remote. Use periodically after merged pull requests.

Review the diff, generated commit message, and pull-request description before approving a push or merge.

### 2. Feature Development

**Use it for:** an ambiguous or multi-file feature that needs discovery, clarifying questions, architecture options, explicit approval, implementation, quality review, and summary.

**Install**

```text
/plugin install feature-dev@claude-plugins-official
```

**Command:**

```text
/feature-dev Add rate limiting to API endpoints
```

`/feature-dev` starts the full seven-phase workflow: discovery, exploration, clarification, architecture, approved implementation, quality review, and summary. Use it when requirements are unclear or the change crosses multiple files. It is not for a one-line fix or urgent hotfix.

### 3. Code Review

**Use it for:** non-trivial pull-request review. It runs parallel reviewers for project-guideline compliance, likely defects, and relevant history, then filters low-confidence findings.

**Install**

```text
/plugin install code-review@claude-plugins-official
```

**Command:** `/code-review`.

Run it from a meaningful pull-request branch when you want several independent reviewers to check the diff, project guidance, obvious defects, and relevant history. Use it before requesting human review, then address and verify confirmed findings.

Review its findings as input to human code review; the plugin does not approve or merge the pull request.

### 4. PR Review Toolkit

**Use it for:** targeted review of test coverage, comments, silent failures, type design, general quality, or code simplification.

**Install**

```text
/plugin install pr-review-toolkit@claude-plugins-official
```

**Invocation:** this plugin uses automatic specialist agents rather than a slash command.

- "Review test coverage for this PR": checks behavior coverage, edge cases, and test gaps.
- "Check for silent failures": reviews catch blocks, fallbacks, and missing error handling.
- "Analyze the type design in this diff": checks invariants, encapsulation, and usefulness of new types.
- "Simplify this code": looks for needless complexity while preserving behavior.

There is no plugin-specific slash-command surface for this toolkit. Use focused natural-language requests so Claude selects the appropriate specialist agent.

### 5. Security Guidance

**Use it for:** continuous security reminders while Claude edits code, diff review when a turn ends, and deeper review when committing.

**Install**

```text
/plugin install security-guidance@claude-plugins-official
```

**Invocation:** automatic hooks; there is no primary slash command.

- During edits, it warns about common dangerous patterns such as hardcoded secrets or unsafe deserialization.
- At the end of a turn, it reviews the changed diff and returns high-severity security findings for Claude to address.
- On commit, it can perform a deeper agentic review that follows data flow across files.

Add project-specific guidance in `.claude/claude-security-guidance.md` for rules the codebase requires.

It complements SAST, DAST, dependency scanning, and human review; it is not a security guarantee.

There is no primary slash command because this plugin runs its checks through edit, stop, and commit hooks.

### 6. Claude Security

**Use it for:** verified vulnerability scans of a repository, changed branch, pull request, or commit; it can also draft reviewable patch files.

**Install**

```text
/plugin install claude-security@claude-plugins-official
/reload-plugins
```

**Command:** `/claude-security`.

Choose the task that matches the work:

- **Scan codebase:** assess a whole repository or a selected directory before a security initiative or major release.
- **Scan changes:** assess a branch, pull-request diff, or one commit before merge.
- **Suggest patches:** turn findings from a current report into separate patch files for review and application.

Inspect the generated report and apply a patch only after normal review and testing.

### 7. CLAUDE.md Management

**Use it for:** finding stale project guidance and converting important session learnings into durable, concise repository memory.

**Install**

```text
/plugin install claude-md-management@claude-plugins-official
```

**Invocation:**

- Ask "Audit my CLAUDE.md files" or "Check if my CLAUDE.md is up to date" to compare project guidance with the codebase and identify stale or missing guidance.
- Run `/revise-claude-md` at the end of a useful session to capture a repeated correction, key command, or project lesson that should survive future sessions.

**Command:** `/revise-claude-md`.

### 8. Hookify

**Use it for:** creating a local warning or block rule from plain-language policy without hand-writing hook configuration.

**Install**

```text
/plugin install hookify@claude-plugins-official
```

**Commands:**

- `/hookify <rule>`: create a warn or block rule from a natural-language instruction.
- `/hookify:list`: show the active local rules and their status.
- `/hookify:configure`: enable or disable existing rules interactively.
- `/hookify:help`: show rule syntax, event types, and examples.

Example: `/hookify Warn me when I use rm -rf commands`. Use blocking only for well-understood patterns; CI and permission policy remain the shared boundary.

### 9. Plugin Development Toolkit

**Use it for:** creating a reusable plugin with commands, skills, agents, hooks, settings, or MCP integration.

**Install**

```text
/plugin install plugin-dev@claude-plugins-official
```

**Command:**

```text
/plugin-dev:create-plugin A plugin for reviewing database migrations
```

`/plugin-dev:create-plugin` guides discovery, component planning, design, implementation, validation, testing, and documentation for a new reusable plugin. Its automatic skills also respond to focused requests such as "Create a PreToolUse hook," "Add an MCP server to my plugin," or "Create a slash command with arguments."

**Available named skills:** `/plugin-dev:hook-development`, `/plugin-dev:mcp-integration`, `/plugin-dev:plugin-structure`, `/plugin-dev:plugin-settings`, `/plugin-dev:command-development`, `/plugin-dev:agent-development`, and `/plugin-dev:skill-development`.

Use a named skill when you know the component you need; use `/plugin-dev:create-plugin` for the complete guided workflow.

### 10. MCP Server Development

**Use it for:** designing an MCP server only when a controlled external integration is genuinely needed.

**Install**

```text
/plugin install mcp-server-dev@claude-plugins-official
```

**Commands:** `/mcp-server-dev:build-mcp-server`, `/mcp-server-dev:build-mcp-app`, and `/mcp-server-dev:build-mcpb`.

- `/mcp-server-dev:build-mcp-server`: choose a deployment model and build a server from the integration requirements.
- `/mcp-server-dev:build-mcp-app`: add an interactive MCP application with in-chat UI widgets.
- `/mcp-server-dev:build-mcpb`: package a local stdio server and its runtime for distribution.

Use the first command before hand-authoring a server when you need help with tool design, authentication, security, and testing. It can also trigger from: "Help me build an MCP server."

### Also install the code-intelligence plugin for the repository language

The official marketplace provides LSP plugins such as `typescript-lsp`, `pyright-lsp`, `csharp-lsp`, `gopls-lsp`, and `rust-analyzer-lsp`. They do not add slash commands; after the required language-server binary is installed, Claude automatically gets diagnostics and precise navigation such as definitions, references, symbols, and implementations.

```text
/plugin install typescript-lsp@claude-plugins-official
/plugin install pyright-lsp@claude-plugins-official
```

LSP plugins have no direct command surface. Ask Claude to find references, jump to a definition, list symbols, or fix reported diagnostics; the language server provides the capability in the background.

### Keep the plugin surface intentional

- Install only plugins that match the repository's work. Review publisher, source, tools, hooks, credentials, MCP scope, update behavior, and data handling before enabling one.
- Disable unused plugins and MCP servers from `/plugin` or `/mcp`; they can add context, local processes, external access, or maintenance overhead.
- Use project scope only after the team agrees on the workflow. Treat plugin upgrades like dependency updates: inspect the change, test it, and keep CI and human review as the final controls.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Permission modes](https://code.claude.com/docs/en/permission-modes)
- [Permissions](https://code.claude.com/docs/en/permissions)
- [Memory](https://code.claude.com/docs/en/memory)
- [Plugins](https://code.claude.com/docs/en/plugins)
- [Plugin marketplace discovery](https://code.claude.com/docs/en/discover-plugins)
- [Official Claude Code plugin directory](https://github.com/anthropics/claude-plugins-official)

[← Previous slide](slide-16-the-commands-you-will-actually-type.md) · [Deck index](../README.md)
