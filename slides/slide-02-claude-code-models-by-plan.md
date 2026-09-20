# Slide 2: Claude Code models by plan

### Tiers & models on the left | Real-world eng. team analogy on the right | Same decision pattern

## Subscription tiers

- Pro — $17/mo annual ($20 monthly) — Sonnet 5 default, Opus available
- Max — From $100/mo — 5x or 20x usage, Opus 5 default, all models
- Team — $20 Standard / $100 Premium per seat — admin controls, SCM
- Enterprise — $20/seat/mo + usage — SSO, RBAC, audit logs, 500k context

## Model lineup

- Haiku 4.5 — Fast specialist — 200k context — $1/$5 per MTok — fastest model, near-frontier intelligence. Ideal for subtasks, triage, log scanning, and parallel tasks.
- Sonnet 5 — Balanced engineer — 1M context — $2/$10 per MTok — best balance of speed and intelligence. Well-suited for routine debugging and everyday work.
- Opus 5 — Default engineer — 1M context — $5/$25 per MTok — start here. Complex agentic coding, architecture decisions, and multi-hour autonomous sessions.
- Fable 1.5 — Principal architect — 1M context — $10/$50 per MTok — most capable model. Escalate for multi-hour design and deep synthesis.

## Team structure

- Sr. Eng. Manager (Rahul)
- Eng. Mgr A (Priya)
- Architect (Amit)
- Eng. Mgr B (Sara)
- 5 Engineers
- 5 Engineers

## Incoming tasks

- Release v2.3 to Production — cross-team coordination needed
- Design Payment Gateway — architecture & trade-off analysis
- Migrate to Kubernetes? — evaluate effort, risk, timeline
- JIRA-1234: Fix login bug — standard bug fix

## SR. manager decides

- Delivery task? — Manager reads the release
- Architecture / Design? — Route to the Architect
- Tech stack decision? — SM decides; Architect input
- Story / Bug? — Managers delegate to engineers

## Role → model mapping

- Engineers → Haiku 4.5 (fast subtasks) · Sonnet 5 (specified work)
- Eng. Managers → Opus 5 (default — start here, everyday coding)
- Architect → Fable 1.5 (long-horizon design & deep synthesis)

### Operating rule

Start at Opus 5 → Fable 1.5 for long-horizon work → Sonnet 5 / Haiku 4.5 for cost & speed. Tune effort before switching models.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Claude plans](https://claude.com/pricing)
- [Claude models overview](https://platform.claude.com/docs/en/about-claude/models/overview)

[← Previous slide](slide-01-claude-code-for.md) · [Deck index](../README.md)