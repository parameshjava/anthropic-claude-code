# Slide 33: Command output stays in the conversation

### Shell output becomes part of the active context

- Command output is evidence, but it is also context. A 10,000-line build log can crowd out the code, constraints, and test result that Claude needs for the next decision.

| Need | Prefer | Avoid |
| --- | --- | --- |
| Find one failure | Focused test or filtered assertion output | Running the full suite and pasting every passing test |
| Diagnose a large log | Save it to a file; ask Claude to search specific errors | Replaying the entire log in the conversation |
| Report completion | Command, exit code, failing/passing assertion, concise summary | "Tests pass" with no evidence |
| Investigate broadly | Delegate log processing to a subagent | Filling the main implementation context with raw output |

### Example

Run a focused test with a concise reporter. Preserve the failing assertion and final pass result in the conversation; keep the complete diagnostic log on disk for targeted follow-up reads if needed.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Context window](https://code.claude.com/docs/en/context-window)
- [Prompt caching](https://code.claude.com/docs/en/prompt-caching)
- [Claude Code costs](https://code.claude.com/docs/en/costs)

[← Previous slide](slide-32-claude-md-is-for-what-every-task-needs.md) · [Deck index](../README.md)