# Slide 5: What this session covers

### Six connected practices for reliable AI engineering

### 1. Choose the right model and effort

- Match capability to uncertainty and blast radius, not just task size. Start with Opus for most engineering work; step down when the problem becomes routine or escalate to Fable for unusually long-horizon synthesis.
- Opus: ambiguous architecture, multi-file defects, or API and frontend changes. Sonnet: well-scoped implementation and routine fixes. Haiku: log scans, triage, boilerplate, and parallel subtasks.
- Example: use Haiku to classify 500 test failures, Sonnet to repair a contained service, and Opus to decide whether the pattern represents a design flaw.

### 2. Set permission rules and safe lanes

- Permission mode decides when Claude must ask. Choose Manual for sensitive repositories, Plan for discovery, and a more permissive mode only where the task and environment are bounded.
- Put durable allow, ask, and deny rules in `.claude/settings.json`. Permission rules are enforced by Claude Code; instructions in a prompt or `CLAUDE.md` are guidance, not a security boundary.
- Example: pre-approve test and diff commands, require approval for database migrations, and block destructive commands or production configuration edits.

### Example `.claude/settings.json`

```json
{
  "permissions": {
    "defaultMode": "plan",
    "allow": [
      "Read",
      "Glob",
      "Grep",
      "Edit",
      "Bash(npm test *)",
      "Bash(git status)",
      "Bash(git diff *)"
    ],
    "ask": ["Bash(dotnet ef database update *)"],
    "deny": [
      "Bash(rm -rf *)",
      "Bash(git push --force *)",
      "Bash(git reset --hard *)",
      "Bash(terraform destroy *)",
      "Bash(curl * | bash)"
    ]
  }
}
```

### 3. Manage context deliberately

- The context window holds the task, conversation, file reads, tool output, and project guidance. Unrelated logs and broad exploration reduce the room available for correct reasoning.
- Give Claude the relevant files with `@` references, keep root `CLAUDE.md` lean, and use directory-level `CLAUDE.md` or path-scoped rules for layer-specific guidance.
- Use `/context` to inspect loaded information, `/compact` to preserve a long task's critical decisions, and `/clear` between unrelated tasks.
- Example: an authentication fix references `@src/controllers/auth.ts`, `@src/services/authService.ts`, and the targeted test instead of asking Claude to scan the whole repository.

### 4. Prompt with an engineering specification

- A strong prompt names the goal, scope, relevant paths, constraints, and verification. This prevents unplanned changes and gives Claude a definition of done.
- State boundaries explicitly: what not to modify, which behavior must remain unchanged, and when Claude must stop for approval.
- Example: "Fix token expiry in @src/services/authService.ts. Do not change the public API. Add a regression test, run the auth suite, and report the command output and diff."

### 5. Delegate isolated work, retain decisions

- Use subagents for noisy, parallel, or specialized tasks: log analysis, repository discovery, security review, or a broad test run. Their findings return as a summary instead of filling the main session with raw output.
- The main session keeps the accepted plan, trade-offs, and final edits. Delegation improves throughput; it does not transfer accountability.
- Example: one subagent maps the token flow, another runs the full auth suite, and the main session applies the smallest fix and reviews the resulting diff.

### 6. Connect only approved tools and integrations

- MCP servers, hooks, CLI tools, and CI workflows can extend Claude beyond the repository. Treat every connection as a new permission, credential, and data-boundary decision.
- Prefer project-reviewed integrations, least-privilege access, and narrowly scoped tool rules. Use hooks for deterministic checks that must happen every time.
- Example: allow a GitHub MCP server to read issues, but keep production deployment credentials outside the agent's reach and require CI plus human approval to release.

### One change, six pillars

For a database migration: choose Opus for the cross-layer design; start in Plan mode with migration commands set to ask; reference the controller, service, repository, and migration files; specify rollback and test requirements; delegate impact analysis; and use only approved database and CI integrations.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Permission modes](https://code.claude.com/docs/en/permission-modes)
- [Permissions](https://code.claude.com/docs/en/permissions)
- [Memory](https://code.claude.com/docs/en/memory)
- [Claude models overview](https://platform.claude.com/docs/en/about-claude/models/overview)

[← Previous slide](slide-04-epic-decomposition-cheat-sheet.md) · [Deck index](../README.md)