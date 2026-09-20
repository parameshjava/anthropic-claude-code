# Slide 7: Four disciplines, one idea

### An agent is powerful only when its controls are written down and enforced

### 1. Permissions: protect the environment

- Start in the least permissive mode that still lets the task move: Manual for sensitive work, Plan for discovery, and broader modes only in bounded environments.
- Use `.claude/settings.json` to express durable allow, ask, and deny rules. Deny and ask rules are enforced by Claude Code; `CLAUDE.md` changes behavior but is not a hard security boundary.
- Use sandboxing and PreToolUse hooks for controls that must hold regardless of model judgment, such as protecting production files or blocking unreviewed migrations.
- Example: let Claude run tests, but require human approval for deploys, pushes, and migration commands.

```json
{
	"permissions": {
		"allow": ["Bash(dotnet test *)"],
		"ask": ["Bash(dotnet ef database update *)"],
		"deny": ["Bash(git push *)", "Edit(/**/appsettings.Production.json)"]
	}
}
```

### 2. Context: protect the task

- Claude works from a finite context window. Long logs, unrelated investigations, and one-off chat instructions compete with the code and constraints it needs to reason well.
- Keep root `CLAUDE.md` concise: architecture, key commands, non-obvious conventions, and team-wide boundaries. Store layer-specific rules beside the layer or in path-scoped `.claude/rules/` files.
- Use `/context` to inspect what is loaded, `/compact` with explicit preservation instructions for long work, and `/clear` between unrelated tasks.
- Example: `src/controllers/CLAUDE.md` can state that controllers validate requests and map HTTP responses, but never access repositories directly. That guidance reloads when Claude works in the controller directory.

### 3. Quality: protect the outcome

- A plausible diff is not proof. Give Claude an objective referee: focused unit tests, integration tests, a build, linting, type checking, UI screenshots, or a behavior comparison.
- Set an explicit quality bar for the change. For business-critical service logic, target at least 90% unit-test coverage where meaningful, while testing error paths, authorization, null handling, cancellation, and boundary conditions.
- Coverage is a signal, not a guarantee. Tests must assert behavior rather than merely execute lines; add integration tests for controllers and repositories where wiring, serialization, or data access matters.
- Keep the design testable: controllers stay thin, services follow the single-responsibility principle, repositories own data access, and cross-layer dependencies remain explicit.
- Example completion evidence: `dotnet test`, coverage report, `dotnet build`, linter output, and the regression test that failed before the fix and passes after it.

### 4. Accountability: protect the business

- The engineer owns the intent, scope, and consequences of the change. Claude can research, implement, and report evidence; it cannot accept product, security, compliance, or operational risk on the team's behalf.
- Code assistants exist to accelerate development, not to replace engineering judgment. Every pull request, approval, deployment, and merged change remains entirely the responsible engineer's and team's accountability.
- Use Plan mode for cross-cutting changes. Review the affected layers, migration strategy, rollout, rollback, and verification plan before allowing edits.
- Treat an AI-generated pull request like any other contribution: review the diff, challenge assumptions, inspect tests, and require the appropriate code-owner or architectural approval.
- Example: before changing authentication, a human approves the proposed token-flow design; before merging, a reviewer confirms the security tests, migration safety, and rollback plan.

### The rule of thumb

Chat instructions are useful but transient. The rule that must survive compaction, a teammate, or a long-running workflow belongs in a source-controlled artifact: settings, hooks, `CLAUDE.md`, rules, skills, CI, or a pull-request process.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Permission modes](https://code.claude.com/docs/en/permission-modes)
- [Permissions](https://code.claude.com/docs/en/permissions)
- [Memory](https://code.claude.com/docs/en/memory)
- [Anthropic engineering: AI code migrations](https://claude.com/blog/ai-code-migration)

[← Previous slide](slide-06-claude-code-operating-model.md) · [Deck index](../README.md)
