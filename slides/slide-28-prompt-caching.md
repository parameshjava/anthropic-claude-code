# Slide 28: Prompt caching

### Reuse a stable prefix instead of recomputing it

- Prompt caching reuses a stable prefix across requests. In practice, stable project guidance, tool configuration, and ongoing task context can be cheaper to reuse than to process as new input each turn.
- Cache behavior depends on provider, model, session activity, and prefix stability. It is an efficiency mechanism, not a correctness or safety control.

### Cache-friendly session habits

- Keep always-loaded instructions concise and stable; avoid repeatedly changing the session's base configuration.
- Continue a focused task while its context is useful; use `/clear` rather than carrying unrelated work into the same prefix.
- Let tool search defer unneeded MCP schemas, and avoid changing tool configuration mid-task without need.
- Use `/usage` or provider reporting to observe cache behavior rather than assuming a hit.

### Example

A sequence of focused auth-fix requests can reuse stable project context. Starting an unrelated documentation task in that session adds noise; clear it and begin a new focused prefix instead. Cache reuse is valuable only when it supports the right work.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Context window](https://code.claude.com/docs/en/context-window)
- [Prompt caching](https://code.claude.com/docs/en/prompt-caching)
- [Claude Code costs](https://code.claude.com/docs/en/costs)

[← Previous slide](slide-27-one-request-one-reply-two-different-kinds-of-passes.md) · [Deck index](../README.md)