# Slide 49: Hooks: automate every step of the agent lifecycle

### Hooks can standardize lifecycle behavior when configured carefully

Hooks run deterministic code at defined lifecycle events. Use them when the requirement is "always do this at this point," not "try to remember this instruction."

| Lifecycle point | Useful hook responsibility |
| --- | --- |
| SessionStart | Prepare safe local context or record the active policy |
| PromptSubmit | Validate or enrich a workflow request where appropriate |
| PreToolUse | Inspect an action and allow, prompt, modify, or block it |
| PostToolUse | Format code, run a focused check, or attach concise result context |
| Stop | Run a completion check or require evidence before the session ends |
| PreCompact | Save a small durable task summary before context is condensed |

### Configuration details that decide whether a hook is a control

- Hook configuration contains event matcher groups, each with its own `hooks` array; validate JSON structure against the current reference before relying on a new policy.
- Command hooks receive structured event JSON on standard input. Parse the relevant `tool_input` field rather than assuming an edited-file environment variable exists.
- For applicable blocking hook decisions, **exit code `2`** is the blocking outcome. Exit code `1` reports an error but is not automatically a policy block.
- Test hooks in a disposable fixture for successful execution, handler failures, matcher coverage, and the exact behavior of every event type you rely on.

### Design hooks as production automation

- Keep each hook narrow, deterministic, observable, and fast. Validate the event payload and scope before acting.
- Never expose credentials in hook output or assume a repository-supplied hook is trusted without workspace review.
- Use a PreToolUse hook for custom runtime checks that permission patterns cannot express. Use CI and branch protection for release controls beyond the local session.
- Do not send large output back into the conversation; return only the evidence needed for the next decision.

### Example

Use a SessionStart hook to surface the approved test command, a PostToolUse hook to format an edited source file, and a PreToolUse hook to block deploy commands outside an approved environment. Keep production release authorization in CI and environment protection.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Hooks guide](https://code.claude.com/docs/en/hooks-guide)
- [Hooks reference](https://code.claude.com/docs/en/hooks)

[← Previous slide](slide-48-scaling-claude-code-across-teams.md) · [Deck index](../README.md)