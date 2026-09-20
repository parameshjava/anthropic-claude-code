# Slide 23: The round trip: a tool call

### Every tool call is a request-and-result cycle

- The model decides it needs evidence and requests a tool action.
- Claude Code applies permissions and runs the action locally or through an approved integration.
- The result returns to the model as fresh context, allowing it to choose the next action.

```text
Model: "Read the failing test"
	↓
Claude Code: checks permissions and reads the file
	↓
Result: test expectation enters the conversation
	↓
Model: decides whether to inspect implementation, edit, or run a focused test
```

### Why this matters

- Tool calls are not incidental. Each one adds latency and context, but they also replace guessing with evidence.
- The goal is not to eliminate tool calls; it is to make every call answer a decision-relevant question.
- Narrow reads and filtered command output produce better next decisions than broad scans and unbounded logs.

### Example

Read the focused test first. If it establishes the expected behavior, inspect the implementation it exercises. After the smallest edit, run that same test before expanding the investigation.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Context window](https://code.claude.com/docs/en/context-window)
- [Prompt caching](https://code.claude.com/docs/en/prompt-caching)
- [Claude Code costs](https://code.claude.com/docs/en/costs)

[← Previous slide](slide-22-it-s-never-just-the-prompt.md) · [Deck index](../README.md)