# Slide 26: The pricing page

### Pricing is a live reference, not a permanent fact

- Input, output, cached input, subscriptions, cloud-provider deployment, and enterprise contracts can each follow different pricing or entitlement rules.
- Use the current official pricing and account documentation for estimates. Slide numbers age; billing and limits change.

### Estimate a workflow, not a single prompt

1. Identify the deployment and billing surface: subscription, API, or cloud provider.
2. Measure the expected model, effort, context size, tool loops, and cache reuse.
3. Compare cost against outcome: time saved, rework avoided, verification completed, and risk reduced.
4. Review actual `/usage` or provider reporting after a pilot; update defaults using evidence.

### Example

Do not choose a model solely because its per-token price is lower. A cheaper model that repeatedly misses the root cause, causes rework, or needs extensive human correction can cost more than a stronger model used once for a difficult design decision.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Context window](https://code.claude.com/docs/en/context-window)
- [Prompt caching](https://code.claude.com/docs/en/prompt-caching)
- [Claude Code costs](https://code.claude.com/docs/en/costs)

[← Previous slide](slide-25-three-kinds-of-tokens.md) · [Deck index](../README.md)