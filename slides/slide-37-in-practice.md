# Slide 37: In practice

### Efficient sessions make deliberate transitions

- Treat a session as a workbench for one coherent problem, not as a permanent inbox for every request.

```text
Frame task → choose model and effort → investigate and implement
	↓
Verify evidence → compact if the same work continues
	↓
Clear before an unrelated task
```

- Choose the model and effort before heavy exploration, then change them only when the evidence shows the task needs more or less reasoning.
- Compact the same task with an explicit list of preserved facts: root cause, modified files, test command, open decisions, and safety boundaries.
- Clear unrelated history before a new feature, bug, or documentation request. Preserve durable knowledge in rules, skills, or issue/PR artifacts, not in stale chat.

### Example

After an auth fix, compact the root cause, regression test, and follow-up decision if the release work continues. Before starting a README update, clear the auth investigation and start with the files and acceptance criteria for documentation.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Context window](https://code.claude.com/docs/en/context-window)
- [Prompt caching](https://code.claude.com/docs/en/prompt-caching)
- [Claude Code costs](https://code.claude.com/docs/en/costs)

[← Previous slide](slide-36-the-cache-expires.md) · [Deck index](../README.md)