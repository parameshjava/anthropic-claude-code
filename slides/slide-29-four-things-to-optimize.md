# Slide 29: Four things to optimize

### Four levers matter more than one raw token count

| Lever | What to optimize | Practical move |
| --- | --- | --- |
| **Context in** | Relevant files, rules, tools, and output | Name files, remove unused MCP, keep root guidance lean |
| **Output out** | The code, explanation, and tool work Claude produces | Request the decision, diff, evidence, or summary actually needed |
| **Model and effort** | Reasoning capability for task uncertainty | Use stronger reasoning for ambiguity; reduce effort for a known local fix |
| **Session shape** | The sequence of turns and transitions | Plan first, compact the same task, clear unrelated work, use subagents for noisy research |

### Optimize the whole loop

- Do not minimize one category at the expense of correctness. Removing a test run may save tokens but can create costly rework.
- Fix the largest source of waste first: broad scans, verbose logs, wrong model defaults, unnecessary retries, or stale session history.
- Use `/context` and `/usage` to observe the actual pressure before changing policy.

### Example

For an auth bug, name the service and test, use a model and effort appropriate for the ambiguity, ask for a focused regression test and result, then clear the session before moving to an unrelated documentation task.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Context window](https://code.claude.com/docs/en/context-window)
- [Prompt caching](https://code.claude.com/docs/en/prompt-caching)
- [Claude Code costs](https://code.claude.com/docs/en/costs)

[← Previous slide](slide-28-prompt-caching.md) · [Deck index](../README.md)