# Slide 34: Effort: how far the model reaches

### Effort changes how much reasoning the model applies

- Effort changes how deeply the selected model reasons before acting. It is often the first control to adjust before switching model families.

| Task | Suitable effort | Why |
| --- | --- | --- |
| Typo, label change, known one-line edit | Low | Expected diff and verification are obvious |
| Well-scoped implementation with focused tests | Medium | Needs implementation reasoning without extended exploration |
| Uncertain root cause, migration, cross-layer design | High | Needs more time for dependencies, risks, and verification planning |

### Use effort deliberately

- Raise effort when the task remains ambiguous after providing the correct files, constraints, and plan.
- Lower it for simple, repeatable work so reasoning spend matches the decision required.
- Effort does not repair a vague prompt or replace tests, plans, permissions, or review.

Use `/effort` or the model controls to adjust it. Example: low effort for a known validation-message change; high effort for a migration that must preserve data, maintain API compatibility, and produce a rollback plan.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Context window](https://code.claude.com/docs/en/context-window)
- [Prompt caching](https://code.claude.com/docs/en/prompt-caching)
- [Claude Code costs](https://code.claude.com/docs/en/costs)

[← Previous slide](slide-33-command-output-stays-in-the-conversation.md) · [Deck index](../README.md)