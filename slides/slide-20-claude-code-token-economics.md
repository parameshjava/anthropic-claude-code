# Slide 20: Claude Code: token economics

### A single request can create many tokens and tool round trips

- The prompt is only the visible trigger. A real session also includes system instructions, project guidance, memory, tool definitions, files, command results, model responses, and sometimes subagent or MCP work.
- A normal agent loop is: model decides → tool runs → result becomes context → model decides again. Each loop improves grounding, but it also adds tokens and latency.

```text
"Fix the failing auth test"
	↓
Load project guidance and tools
	↓
Read auth code and test → inspect failure → edit → run test
	↓
Read result → revise or report evidence
```

### What drives the total cost

| Cost driver           | Good operating habit                            |
| --------------------- | ----------------------------------------------- |
| Startup context       | Keep `CLAUDE.md`, MCP servers, and skills lean  |
| File reads and search | Name known files and avoid broad scans          |
| Command output        | Filter logs and keep only useful evidence       |
| Rework loops          | Plan uncertain changes and verify incrementally |
| Model and effort      | Match reasoning capability to task uncertainty  |

### The point of token economics

Do not optimize for the fewest turns or the shortest answer. Optimize for a correct, reviewable outcome with no wasted exploration, repeated rework, or irrelevant context.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Context window](https://code.claude.com/docs/en/context-window)
- [Prompt caching](https://code.claude.com/docs/en/prompt-caching)
- [Claude Code costs](https://code.claude.com/docs/en/costs)

[← Previous slide](slide-19-manage-context-avoid-limits-force-specificity.md) · [Deck index](../README.md)
