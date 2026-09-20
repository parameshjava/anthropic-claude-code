# Slide 11: Auto mode — the classifier

### Auto mode is a safety gate, not a correctness guarantee

- Auto mode reduces routine prompts by sending actions to a separate safety classifier before they run. The classifier looks for scope escalation, unknown infrastructure, hostile-content-driven actions, data exposure, and destructive behavior.
- It is a gate on actions, not a reviewer of business requirements. It cannot prove that a change meets the specification, preserves architecture, handles edge cases, or is safe to release.
- Auto mode is appropriate for a well-bounded task whose general direction you trust. It is not a substitute for a plan, verification, code review, or release approval.

### How the decision path works

```text
Claude proposes an action
				↓
Explicit deny / ask / allow rules resolve first
				↓
Routine safe actions may proceed
				↓
Other actions go to the classifier
				↓
Allowed, blocked, or prompted according to the result
```

- **Deny** rules block matching actions in every mode. **Ask** rules force a human prompt even in Auto mode. Use these for non-negotiable boundaries.
- The classifier reads the task and project guidance, including `CLAUDE.md`, but guidance influences behavior; it does not replace an enforced permission rule or hook.
- If Auto mode repeatedly blocks actions, it falls back to prompting. Treat repeated blocks as a signal to clarify the task or configure trusted infrastructure, not as proof that the work is wrong.

### What Auto mode can help protect

- It can flag or block suspicious activity such as downloading and executing code, force pushes, destructive resets, production deploys or migrations, destructive infrastructure commands, secret exposure, and disabling security checks.
- It helps reduce approval fatigue for normal repository work, such as local file operations, declared dependency installation, and routine read-only activity.
- It can still allow actions that a team wants reviewed. For example, a routine push may be acceptable to the classifier even when your engineering process requires human approval.

### Put human checkpoints in rules

```json
{
	"permissions": {
		"ask": [
			"Bash(git push *)",
			"Bash(dotnet ef database update *)",
			"Bash(terraform apply *)"
		],
		"deny": [
			"Bash(git push --force *)",
			"Bash(git reset --hard *)"
		]
	}
}
```

- Use `ask` for actions the team must explicitly review: production deploys, migrations, pushes, permission changes, and branch-protection changes.
- Use `deny` for actions the project must never perform from an agent session. Use hooks or sandboxing for deterministic runtime controls.

### Example: bounded Auto-mode workflow

Run an Auto-mode lint-fix or test-fix loop only after defining the allowed scope and the required checks. Let Claude edit the application code and run the test suite, but force approval for migration commands and pushes. The engineer then reviews the test output and final diff before opening or approving the pull request.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Permission modes](https://code.claude.com/docs/en/permission-modes)
- [Permissions](https://code.claude.com/docs/en/permissions)
- [Memory](https://code.claude.com/docs/en/memory)

[← Previous slide](slide-10-plan-mode-look-before-you-touch.md) · [Deck index](../README.md)