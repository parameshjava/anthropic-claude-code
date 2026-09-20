# Slide 32: CLAUDE.md is for what every task needs

### Keep durable, task-wide guidance in the repo root

- Root `CLAUDE.md` loads at session start. Treat each line as a shared, always-on instruction that must earn its context cost.

| Keep in root `CLAUDE.md` | Move elsewhere |
| --- | --- |
| Build, test, lint, and type-check commands | Detailed migration or release procedure → skill |
| Architecture and non-obvious boundaries | One-layer convention → nested `CLAUDE.md` or path rule |
| Team workflow and common pitfalls | Personal preference → `CLAUDE.local.md` or user guidance |
| Facts Claude cannot derive from code | Generated documentation or file-by-file tours |

### Keep it effective

- Aim for concise, specific, version-controlled guidance. Anthropic recommends keeping each `CLAUDE.md` under roughly 200 lines.
- Review it like code: remove stale instructions, resolve conflicts, and move bulky conditional material to on-demand skills or scoped rules.
- Confirm it loaded with `/context`; a rule cannot guide a session if the file is not in its memory list.

### Example

Keep "run the project test command before completing a code change" and the controller-to-service-to-repository boundary in root guidance. Put the full migration review procedure in a migration skill that loads only for schema work.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Context window](https://code.claude.com/docs/en/context-window)
- [Prompt caching](https://code.claude.com/docs/en/prompt-caching)
- [Claude Code costs](https://code.claude.com/docs/en/costs)

[← Previous slide](slide-31-mention-the-file-you-already-know.md) · [Deck index](../README.md)