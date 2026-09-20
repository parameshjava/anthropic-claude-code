# Slide 27: One request, one reply: two different kinds of passes

### Input and output are different phases of the same round trip

- **Input pass:** Claude receives the prompt, project context, tool definitions, prior results, and any newly read files.
- **Output pass:** Claude generates text, code, or a tool request from that input.
- A tool request pauses the visible response. When the tool result returns, the next request has more evidence and a new input pass begins.

```text
Input: prompt + context + test failure
	↓
Output: request to read implementation
	↓
Tool result: implementation source
	↓
New input: prior context + source
	↓
Output: proposed fix or test command
```

### Operational implication

- The cost and quality of a task are determined by the complete sequence of passes, not the length of the opening prompt or final response.
- Make input precise and tool output bounded so each new pass improves the decision rather than repeating noisy context.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Context window](https://code.claude.com/docs/en/context-window)
- [Prompt caching](https://code.claude.com/docs/en/prompt-caching)
- [Claude Code costs](https://code.claude.com/docs/en/costs)

[← Previous slide](slide-26-the-pricing-page.md) · [Deck index](../README.md)