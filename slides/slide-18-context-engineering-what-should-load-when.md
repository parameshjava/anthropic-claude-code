# Slide 18: Context engineering: what should load when

### Good context is selective, not maximal

- Context design is repository architecture for the agent: decide what is always present, what loads for a matching path, and what arrives only when a workflow needs it.
- More context is not automatically better. Prefer the smallest set of facts that lets Claude make the next correct decision.

| Load timing                       | Mechanism                      | Put this information there                                               |
| --------------------------------- | ------------------------------ | ------------------------------------------------------------------------ |
| Every project session             | Root `CLAUDE.md`               | Build commands, architecture, non-obvious conventions, shared boundaries |
| When a directory is touched       | Nested `CLAUDE.md`             | Controller, service, repository, or migration-layer responsibilities     |
| When matching files are read      | `.claude/rules/` with `paths:` | API conventions, test requirements, or language-specific constraints     |
| When a procedure is needed        | Skill or command               | Migration playbook, security review, release workflow                    |
| When an external system is needed | Approved MCP or CLI            | Issue tracker, monitoring, database, or deployment interaction           |

### Design principles

### Three context tiers

| Tier | What belongs there | Example |
| --- | --- | --- |
| Always on | Stable foundation for every task | Root project guidance, user/org instructions, unscoped rules, auto memory |
| On demand | Information tied to a path or workflow | Path rules, nested guidance, skills, `@` references, subagent findings |
| External and automated | Capability beyond the repository | Approved MCP, CLI/API access, hooks, and CI workflows |

Use this structure to avoid placing every useful fact in root `CLAUDE.md`. Stable facts load early; conditional procedures and external capability load only when the task establishes a need.

- Keep root guidance concise and stable. If it only matters to one layer or task, move it down or load it on demand.
- Keep raw logs, generated files, vendor directories, and broad repository scans out of default context unless they are directly relevant.
- Use `/context` to confirm the guidance and integrations that actually loaded; configuration that is not loaded cannot influence the session.

For large monorepos, consider a sparse worktree or targeted worktree checkout so the agent's visible filesystem matches the module being changed. Keep shared paths in project configuration and developer-specific service paths in personal configuration.

### Example

Keep the test command and controller-to-service-to-repository boundary in root guidance. Put migration safeguards under `src/migrations/CLAUDE.md`, and load a full migration-review skill only for schema work.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Permission modes](https://code.claude.com/docs/en/permission-modes)
- [Permissions](https://code.claude.com/docs/en/permissions)
- [Memory](https://code.claude.com/docs/en/memory)
- [Context window](https://code.claude.com/docs/en/context-window)

[← Previous slide](slide-17-plugins-install-the-discipline-don-t-re-remember-it.md) · [Deck index](../README.md)
