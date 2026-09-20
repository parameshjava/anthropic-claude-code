# Slide 22: It's never just the prompt

### The model sees the whole request, not just the typed sentence

- Before the first prompt, Claude Code may already load system instructions, project memory, rules, skill descriptions, MCP tool names, and environment information.
- During a task, file reads, tool results, command output, edits, and follow-up messages become part of the working context. The sentence you type is usually the smallest part of the request.
- This is why a short prompt can still be expensive or unfocused: the model must reason over everything that accompanies it.

### Design for a useful request

- Keep always-loaded guidance concise; move conditional procedures into skills or path-scoped rules.
- Name known files with `@` so Claude begins with the right context rather than spending turns searching.
- Exclude generated output, irrelevant logs, and unused external tools from startup context.
- Use `/context` when a session feels slow or vague; inspect the real context instead of guessing from the visible prompt.

### Example

Better: "Fix the expiry check in @src/auth/token.ts. Use @tests/auth/token.test.ts, preserve the API, and run the focused test."

The model starts with the likely implementation and proof rather than discovering both through a broad repository scan.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Context window](https://code.claude.com/docs/en/context-window)
- [Prompt caching](https://code.claude.com/docs/en/prompt-caching)
- [Claude Code costs](https://code.claude.com/docs/en/costs)

[← Previous slide](slide-21-the-unit-is-tokens-either-way.md) · [Deck index](../README.md)