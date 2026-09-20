# Slide 35: The cache matches from the first token on

### Caching depends on a stable shared prefix

- Prompt caching matches a stable prefix shared by consecutive requests. Stable project instructions, tool configuration, and an ongoing focused task are more reusable than a session that constantly changes its base setup.
- A change early in the request sequence can require the provider to process later material as new input. Cache behavior depends on provider rules and expiry, so observe it rather than assuming savings.

### Keep the shared prefix stable when it helps

- Keep root guidance concise and change it deliberately, not during every turn.
- Do not add or remove MCP servers mid-task unless the work genuinely needs them.
- Continue the same focused task while its context is valuable; use `/clear` for unrelated work instead of polluting the prefix.
- Use `/usage` or provider telemetry to see cache hits, misses, and rebuilds.

### Example

Changing a system instruction or tool configuration near the front can prevent later requests from matching the cached prefix. That is acceptable when needed for correctness; caching should shape hygiene, not prevent necessary configuration changes.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Context window](https://code.claude.com/docs/en/context-window)
- [Prompt caching](https://code.claude.com/docs/en/prompt-caching)
- [Claude Code costs](https://code.claude.com/docs/en/costs)

[← Previous slide](slide-34-effort-how-far-the-model-reaches.md) · [Deck index](../README.md)