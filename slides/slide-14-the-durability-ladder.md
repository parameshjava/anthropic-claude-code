# Slide 14: The durability ladder

### Put the rule where it can actually do its job

Ask two questions before writing a rule:

1. **Does it only guide Claude, or must it stop an action?**
2. **Must it survive this chat, every session, or every pull request?**

The answer decides the right layer. A strongly worded chat message is still only a chat message; it cannot replace a technical control.

### The ladder: from advice to enforcement

| Put it here | Use it for | What it guarantees |
| --- | --- | --- |
| **Conversation** | A one-task preference: "Use this helper for this fix" | Useful now; may disappear after compaction |
| **`CLAUDE.md` or `.claude/rules/`** | Project conventions, architecture, and layer boundaries | Reusable guidance in future sessions; advisory, not enforcement |
| **`permissions.ask`** | Actions a person must approve: pushes, migrations, deploys | Claude Code pauses, even in Auto mode |
| **`permissions.deny` or a PreToolUse hook** | Actions an agent session must not perform | The matching action is blocked at runtime |
| **CI, branch protection, and code review** | Changes that must never be merged or released without proof | The delivery process rejects or holds the change for every contributor |

### 1. Conversation: one-task guidance

Use the prompt for a temporary constraint that applies only to the current work.

```text
Fix the null-reference error in @src/services/InvoiceService.cs.
Use the existing repository helper. Do not refactor unrelated services.
Run the focused unit tests and show the result before you finish.
```

This is fast and useful, but it can disappear after compaction or when another engineer starts a new session.

### 2. `CLAUDE.md` or rules: durable conventions

Put architecture and coding conventions in version-controlled project guidance.

```markdown
# src/services/CLAUDE.md
- Services own business rules and orchestration.
- Do not access DbContext directly; use repositories.
- Add unit tests for every new business-rule branch.
```

This guides every relevant session, but it remains advisory. Do not use it as the only control for an action that must never happen.

### 3. `permissions.ask`: a required human checkpoint

Configure a prompt for actions a human must explicitly approve, even in Auto mode.

```json
{
	"permissions": {
		"ask": [
			"Bash(git push *)",
			"Bash(dotnet ef database update *)",
			"Bash(terraform apply *)"
		]
	}
}
```

Store team-wide policy in `.claude/settings.json`; use user or managed settings when the boundary must apply more broadly.

### 4. `permissions.deny` or hooks: block the action

Use a deny rule for a known prohibited command. Use a PreToolUse hook when the policy needs custom inspection of the full command or environment.

```json
{
	"permissions": {
		"deny": [
			"Bash(git push --force *)",
			"Bash(git reset --hard *)",
			"Edit(/**/appsettings.Production.json)"
		]
	}
}
```

Example hook policy: block any deploy command that targets a production hostname unless an approved release workflow supplies the required environment marker.

### 5. CI and review gates: block an unsafe delivery

Put ship/no-ship checks in the delivery system so they apply to every contributor, with or without an AI tool.

```yaml
name: Verify pull request
on: [pull_request]
jobs:
	verify:
		runs-on: ubuntu-latest
		steps:
			- uses: actions/checkout@v4
			- run: dotnet test --configuration Release
			- run: dotnet build --configuration Release --no-restore
```

Require this status check and code-owner approval in branch protection before merging. Use release approval and environment protection for production deployment.

### A simple rule of thumb

- Put **knowledge and conventions** in `CLAUDE.md` or rules.
- Put **human approval points** in `ask` rules.
- Put **non-negotiable runtime restrictions** in `deny` rules or hooks.
- Put **ship/no-ship requirements** in CI and review gates.

These layers complement one another. For example, a migration can be described in `CLAUDE.md`, require an `ask` prompt before execution, be blocked by a hook outside approved environments, and require CI plus reviewer approval before merge.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Permission modes](https://code.claude.com/docs/en/permission-modes)
- [Permissions](https://code.claude.com/docs/en/permissions)
- [Memory](https://code.claude.com/docs/en/memory)

[← Previous slide](slide-13-a-claim-is-not-evidence.md) · [Deck index](../README.md)