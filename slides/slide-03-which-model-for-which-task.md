# Slide 3: Which model for which task?

### Start with Opus; step down or escalate deliberately

- Before changing model, try a lower effort level first
- Default to Opus for ambiguous, multi-step engineering work
- Step down to Sonnet for well-scoped work with clear requirements
- Use Haiku for fast triage, boilerplate, and parallel subtasks
- Escalate to Fable for long-horizon reasoning and synthesis

### Debugging & fixing

- Opus: stack traces, data-flow tracing, intermittent bugs
- Sonnet: routine bug fixes with a clear root cause
- Haiku: log scans, error summaries, parallel investigations
- Fable: multi-hour, cross-system failures that need deep exploration

### New development

- Opus: ambiguous features, migrations, API + frontend changes
- Sonnet: well-specified feature work and implementation tasks
- Haiku: scaffolding, boilerplate, test matrices, parallel subtasks
- Fable: large autonomous builds that need a sustained plan

### Design & architecture

- Opus: trade-off analysis and strategic system decisions
- Fable: multi-document synthesis, design docs, RFCs, delivery plans
- Sonnet: straightforward design questions with a known shape
- Haiku: summarize specs, extract structured fields, chunk docs

### Key takeaway

Model selection is a per-task decision: start at Opus, step down when the work is clear and cheap, and escalate to Fable only when the reasoning burden is real.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Claude models overview](https://platform.claude.com/docs/en/about-claude/models/overview)
- [Choosing a model](https://platform.claude.com/docs/en/about-claude/models/choosing-a-model)

[← Previous slide](slide-02-claude-code-models-by-plan.md) · [Deck index](../README.md)