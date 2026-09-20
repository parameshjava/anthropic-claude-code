# Slide 39: OpenTelemetry

### Telemetry helps teams improve policy with evidence

- OpenTelemetry can expose model usage, token and cache behavior, session patterns, tool activity, and latency to the observability system the organization already operates.
- Use this data to find workflow friction: expensive defaults, long-context sessions, repeated failures, unused integrations, or policy that does not behave as intended.

### Turn telemetry into improvement, not surveillance theater

1. Define a question: "Are routine test fixes using the strongest model?" or "Which sessions repeatedly miss cache?"
2. Inspect aggregate evidence and representative workflows.
3. Improve guidance, defaults, skills, or training.
4. Measure whether the change improves quality, cost, or developer experience.

- Telemetry is observability, not an approval system. It cannot replace tests, code review, or a permission boundary.
- Apply privacy, retention, least-access, and data-classification controls to telemetry just as you would to application logs.

### Example

Telemetry shows a team running high-effort reasoning for routine formatting and maintaining idle MCP servers. The team changes the default guidance, disables unused integrations, and checks whether quality remains stable while context pressure drops.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Context window](https://code.claude.com/docs/en/context-window)
- [Prompt caching](https://code.claude.com/docs/en/prompt-caching)
- [Claude Code costs](https://code.claude.com/docs/en/costs)
- [Usage monitoring](https://code.claude.com/docs/en/monitoring-usage)

[← Previous slide](slide-38-managed-settings.md) · [Deck index](../README.md)