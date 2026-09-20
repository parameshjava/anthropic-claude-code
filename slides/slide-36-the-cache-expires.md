# Slide 36: The cache expires

### Reuse is temporary, not permanent

- Cached prefixes have a lifetime. After enough idle time, the provider may need to process the context as new input again.
- Exact lifetime and pricing depend on the provider, model, account, and configured cache policy. Do not hard-code assumptions from an old slide or another deployment.

### What to do when work resumes

- Resume only when the earlier conversation is still the right context for the task.
- If the task changed, use `/clear` and begin a focused session instead of paying to carry a stale transcript.
- Use `/usage` and provider reporting to identify cache misses and long-context behavior; optimize the observed workflow rather than guessing.

### Example

An engineer returns after a long break to an auth investigation. Resume if the next task is still the same root-cause analysis; otherwise preserve the needed conclusion in a short prompt or source-controlled note and start fresh for unrelated work.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Context window](https://code.claude.com/docs/en/context-window)
- [Prompt caching](https://code.claude.com/docs/en/prompt-caching)
- [Claude Code costs](https://code.claude.com/docs/en/costs)

[← Previous slide](slide-35-the-cache-matches-from-the-first-token-on.md) · [Deck index](../README.md)