# Slide 48: Scaling Claude Code across teams

### Scale from individual technique to shared operating system

Scaling succeeds when the safe, evidence-based workflow is easier to repeat than an improvised prompt.

### Start with a minimum repository standard

- A lean root `CLAUDE.md`: architecture, key commands, non-obvious conventions, ownership, and boundaries.
- Directory guidance or `.claude/rules/` for layer-specific behavior.
- One clear verification command and pull-request expectation for changes.
- A project `.claude/settings.json` that expresses the repository's safe defaults and human checkpoints.

### Then package what repeats

| Repeated team need | Shared mechanism |
| --- | --- |
| Migration or security review | Skill with a repeatable checklist and evidence format |
| Large investigation or review | Specialized read-only subagent |
| Formatting or protected-file check | Hook |
| Issue tracker or monitoring access | Reviewed MCP/CLI integration |
| Organization-wide boundary | Managed settings and CI policy |

### Scale with feedback, not a one-time rollout

1. Pilot on a representative repository with named engineering and security owners.
2. Measure adoption, verification quality, rework, cost, and policy friction.
3. Turn successful patterns into source-controlled skills, rules, and templates.
4. Review and prune them regularly so guidance stays useful rather than becoming noise.

### Example

Standardize a small root guide and one verification command first. Add a reviewed `migration-review` skill and a security reviewer only after teams repeatedly need them; enforce organization-wide restrictions through managed settings and CI.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Administration](https://code.claude.com/docs/en/admin-setup)
- [Managed settings](https://code.claude.com/docs/en/managed-settings)

[← Previous slide](slide-47-enterprise-deployment-and-governance.md) · [Deck index](../README.md)