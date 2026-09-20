# Slide 52: Anthropic Claude Code

### Final question: what will your team actually adopt?

Claude Code accelerates engineering work when the team supplies clear context, bounded capabilities, verification, and accountable human decisions.

### Leave with four concrete decisions

1. **Planning:** Which changes require Plan mode before edits, such as migrations, auth, infrastructure, or cross-layer refactors?
2. **Boundaries:** Which commands, paths, integrations, and release actions require `ask`, `deny`, hooks, CI, or branch protection?
3. **Durable guidance:** What belongs in root and directory-level guidance, rules, and repeatable skills?
4. **Proof:** Which tests, builds, contract checks, screenshots, or reviews establish that a change is done?

### A practical pilot

| Pilot decision | Start small |
| --- | --- |
| Repository | One representative, non-critical service |
| Safe defaults | Plan mode, reviewed `.claude/settings.json`, lean `CLAUDE.md` |
| Quality bar | Focused tests, build, diff review, and pull-request approval |
| Measure | Rework avoided, verification rate, developer friction, and usage |
| Owner | One engineering owner and one security or platform partner |

### First-week checklist

| Time | Minimum outcome |
| --- | --- |
| Days 1-2 | Give a pilot group access; add a reviewed root `CLAUDE.md` with architecture and test commands |
| Days 3-4 | Add a few path-scoped rules for common risk areas; approve only integrations with a demonstrated need |
| Day 5 | Run a working session on prompt scope, Plan mode, context controls, evidence, and diff review |
| After the pilot | Add hooks, custom agents, and CI automation only where repeated evidence shows they improve the workflow |

Measure matched work against a baseline: cycle time, review rework, escaped defects, meaningful coverage, adoption, and developer experience. Do not promise a universal productivity multiplier.

### The operating rule

Use the tool to speed up development. Engineers remain responsible for every plan, pull request, approval, deployment, and merged change. Build the workflow so that responsibility is supported by evidence and enforceable controls, not replaced by automation.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)

[← Previous slide](slide-51-claude-code-security-reasoning-based-vulnerability-scanning.md) · [Deck index](../README.md)