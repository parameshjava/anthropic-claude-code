# Slide 25: Three kinds of tokens

### The cost model has three main inputs to track

- **Input tokens:** what the model receives: system instructions, prompt, prior conversation, files, tool schemas, and tool results.
- **Output tokens:** what the model generates: analysis, text, code, and tool-call requests.
- **Cached input tokens:** stable prior input reused under the provider's cache rules rather than processed as new input.

| Token category | Main lever | Avoid optimizing it by |
| --- | --- | --- |
| Input | Relevant files, concise guidance, focused output | Removing facts needed for correctness |
| Output | Task-appropriate model and effort, explicit requested format | Forcing short answers when reasoning or evidence is needed |
| Cached input | Stable session prefix and active work cadence | Assuming cache reuse is a security or correctness control |

### Example

The prompt and prior transcript are input; the patch and tool call are output; stable project guidance and conversation prefix may be cached. The best optimization is to remove irrelevant context while keeping the tests, constraints, and evidence required to make the next decision correctly.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Context window](https://code.claude.com/docs/en/context-window)
- [Prompt caching](https://code.claude.com/docs/en/prompt-caching)
- [Claude Code costs](https://code.claude.com/docs/en/costs)

[← Previous slide](slide-24-one-prompt-five-requests.md) · [Deck index](../README.md)