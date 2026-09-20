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

- Keep root guidance concise and stable. If it only matters to one layer or task, move it down or load it on demand.
- Keep raw logs, generated files, vendor directories, and broad repository scans out of default context unless they are directly relevant.
- Use `/context` to confirm the guidance and integrations that actually loaded; configuration that is not loaded cannot influence the session.

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
