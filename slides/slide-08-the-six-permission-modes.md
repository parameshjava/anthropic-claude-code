# Slide 8: The six permission modes

### Permission mode is policy, not capability

- A permission mode determines which tool calls Claude Code can run without asking; it does not grant access that a deny or ask rule has restricted.
- Use the least permissive mode that lets the task progress. Move deliberately from discovery to implementation rather than treating approval prompts as friction to remove.

### The six modes

| Display mode | Config value | Runs without asking | Use it when |
| --- | --- | --- | --- |
| Manual | `default` | Reads only | The repo or change is sensitive or unfamiliar |
| Accept edits | `acceptEdits` | Reads, file edits, and common filesystem operations | You are watching a small, bounded implementation |
| Plan | `plan` | Reads and approved exploration commands | You need a design, impact analysis, or migration plan before edits |
| Auto | `auto` | Classifier-approved actions | A long, well-bounded task needs fewer interruptions |
| Don't ask | `dontAsk` | Reads and pre-approved tools only | A locked-down CI or scripted workflow has no person to answer prompts |
| Bypass permissions | `bypassPermissions` | Nearly all actions, without approval prompts | A disposable container or VM only |

### Two modes that need extra care

- **Don't ask is deny-by-default, not approve-by-default.** Anything not explicitly allowed is denied instead of prompting, which makes it suitable for unattended CI.
- **Bypass permissions removes approval checks.** It can write protected locations such as `.git` and `.claude`; use it only in an isolated environment that can be discarded.
- Permission rules remain the durable boundary. Use `.claude/settings.json` to allow routine checks, ask before risky actions, and deny actions your project must never perform.

### Two operational gotchas

- There is no Claude Code "safe mode." The UI calls the review-every-action mode **Manual**; its configuration value remains `default`.
- The interactive mode cycle is not a recommended workflow order: `Manual → Accept edits → Plan → Manual`, with available optional modes after Plan. Confirm the status bar rather than assuming a session's mode.
- Project `.claude/settings.json` can set `defaultMode` to values such as `plan`, but `auto` and `bypassPermissions` do not become terminal defaults from project or local settings. Set organization/user policy at the correct scope and verify it with `/status`.

### Recommended lifecycle for an engineering change

```text
Manual (optional trust check)
	↓
Plan (explore, map impact, agree the design)
	↓
Accept edits (implement while watching the diff)
	↓
Auto (optional: bounded test/fix loop)
	↓
Manual (review evidence, final diff, and merge decision)
```

- **Manual first:** use it for an unfamiliar repository, production investigation, or any task where the initial scope is unclear. Confirm the working directory, effective settings, and risky boundaries.
- **Plan next:** map the controller, service, repository, test, and migration impact before source files change. A human approves the proposed approach before implementation begins.
- **Accept edits for implementation:** let Claude make the reviewed changes and focused tests while you monitor the diff. Keep `ask` rules around migrations, deploys, pushes, and other consequential operations.
- **Auto only when bounded:** use it for a contained lint or test-fix loop after deny rules protect destructive actions, production configuration, and scope expansion.
- **Return to Manual for completion:** inspect test output, coverage or build evidence, the final diff, and the pull request before the engineer accepts the result.

### Special-purpose branches, not lifecycle steps

- **Don't ask:** use in CI or scripts after pre-approving only the exact build, test, and reporting commands the workflow needs. It denies everything else.
- **Bypass:** use solely for throwaway experiments in an isolated VM or container. It is never a normal next step for a shared development checkout.

### Start a session intentionally

```bash
claude --permission-mode plan
```

Use `Shift+Tab` to move among the interactive modes, and check the status bar before consequential work. The UI switching order is not the recommended engineering lifecycle. Keep destructive actions such as force pushes, hard resets, production deploys, and unreviewed migrations behind explicit `ask` or `deny` rules.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Permission modes](https://code.claude.com/docs/en/permission-modes)
- [Permissions](https://code.claude.com/docs/en/permissions)
- [Memory](https://code.claude.com/docs/en/memory)

[← Previous slide](slide-07-four-disciplines-one-idea.md) · [Deck index](../README.md)