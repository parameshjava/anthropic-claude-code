# Slide 44: Which feature solves which problem?

### Use the lightest mechanism that matches the problem

| Problem | Best mechanism | Why |
| --- | --- | --- |
| Every session needs a convention or command | `CLAUDE.md` | Durable, project-wide guidance |
| One path or file type needs a rule | `.claude/rules/` or nested `CLAUDE.md` | Loads only where it is relevant |
| A repeatable multi-step procedure | Skill or command | Loads on demand; keeps root context lean |
| Noisy research or specialized review | Subagent | Isolated context and role; returns a summary |
| A deterministic lifecycle check | Hook | Runs at a fixed event rather than relying on a model instruction |
| An approved external system | MCP or CLI/API integration | Controlled access to data and actions outside the repo |
| A rule that must block an action | Permission deny/ask or CI gate | Enforced boundary, not advisory guidance |

### Example: migration work

- Root `CLAUDE.md`: architecture and project test commands.
- `src/migrations/CLAUDE.md`: forward-only migration conventions.
- `migration-review` skill: repeatable review and rollback workflow.
- Subagent: read-only impact analysis across repositories.
- Hook or permission rule: require approval before applying a migration.
- CI: block merge until migration tests and reviewer approval pass.

Choose the smallest mechanism that gives the right durability and enforcement. Do not build a plugin or MCP server for a one-line project rule.

### Skill versus agent versus MCP

| Need | Choose | Why |
| --- | --- | --- |
| Guide the engineer through TDD, debugging, or documentation | Skill | Methodology stays in the main session |
| Run tests, analyze logs, or review a diff without adding noise | Subagent | Separate context returns a concise result |
| Read or change an issue tracker, database, or dashboard | MCP or CLI/API | Provides the external capability an agent or skill can use |

Start a reusable workflow as a local `.claude/skills/<name>/SKILL.md`. Package it as a plugin only when it is stable enough to distribute across projects and teams.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Skills](https://code.claude.com/docs/en/skills)

[← Previous slide](slide-43-reduce-hallucinations-and-unsafe-edits.md) · [Deck index](../README.md)