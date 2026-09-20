# Slide 21: The unit is tokens, either way

### Usage is measured by the product and plan you are actually on

- Subscription allowances, API billing, cloud-provider billing, and enterprise spend controls are different commercial models. Do not treat a price from one surface as a universal rate.
- Token limits, context windows, cache behavior, model availability, and provider features can change. Check the current official documentation and your organization’s contract before estimating cost or setting policy.

| Question                                 | Source of truth                                             |
| ---------------------------------------- | ----------------------------------------------------------- |
| What does this developer's plan include? | Current plan and account usage information                  |
| What will an API workflow cost?          | Claude Console usage and current API pricing                |
| What is cloud-provider spend?            | AWS, Google Cloud, or Microsoft billing and budget controls |
| What limits apply to this organization?  | Managed settings, provider limits, and organization policy  |

### Make cost discussions useful

- Measure the real workflow: model, effort, context size, tool calls, cache behavior, and rework rate.
- Review `/usage` and organization reporting before changing defaults. A long context or an expensive default model may be the cause; the fix is not automatically a cheaper model.
- Treat deck pricing as a discussion aid, never as an invoice, contract, or permanent architecture decision.

### Example

Before recommending a Team or Enterprise budget, validate the current entitlement, provider path, internal rate, and expected workflow. Then set an explicit spend limit while keeping independent permission and review controls for safety.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Context window](https://code.claude.com/docs/en/context-window)
- [Prompt caching](https://code.claude.com/docs/en/prompt-caching)
- [Claude Code costs](https://code.claude.com/docs/en/costs)

[← Previous slide](slide-20-claude-code-token-economics.md) · [Deck index](../README.md)
