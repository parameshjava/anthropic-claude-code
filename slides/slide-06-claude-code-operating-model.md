# Slide 6: Claude Code operating model

### Claude Code is an agent with context, tools, memory, delegation, and verification

- Context: CLAUDE.md, rules, @-mentions, and /context tell it what matters
- Tools: Read, Edit, Write, Bash, Git, hooks, and MCP extend the agent
- Memory: auto memory and explicit project recall keep useful knowledge across sessions
- Delegation: subagents, custom agents, and agent teams isolate noisy or parallel work
- Verification: the main session keeps decisions; the model must prove correctness with evidence

### The loop

- Receive the task and the constraints
- Inspect the repo and the relevant files
- Act with focused reads, edits, and commands
- Delegate noisy or parallel work when it adds value
- Verify with tests, diffs, and clear output
- Keep the final engineering decision with the human

### Keep CLAUDE.md lean; place rules next to the code they govern

- Anthropic supports global, project, and subdirectory CLAUDE.md files
- The root project file is loaded in every session, so keep it short and broadly useful
- For large repos and layered apps, subdirectory CLAUDE.md files keep context focused
- Use root CLAUDE.md for architecture, commands, and team-wide conventions only
- Use folder-level CLAUDE.md files for API, service, DB, or migration boundaries

### Recommended structure for a layered API

```text
API/
├── CLAUDE.md                     # architecture, commands, shared conventions
├── src/
│   ├── controllers/
│   │   └── CLAUDE.md             # endpoint validation, auth, HTTP boundaries
│   ├── services/
│   │   └── CLAUDE.md             # business rules and workflow orchestration
│   ├── repositories/
│   │   └── CLAUDE.md             # DB access, transactions, query rules
│   └── migrations/
│       └── CLAUDE.md             # schema practices and safety checks
└── .claude/
	├── settings.json           # project-scoped permissions and allowed tools
	├── rules/
	│   ├── api-design.md         # API-wide design conventions
	│   ├── controllers.md        # controller-specific standards
	│   ├── services.md           # service-layer standards
	│   ├── repositories.md       # data-access standards
	│   └── migrations.md         # schema-change safeguards
	├── skills/
	│   ├── api-review/
	│   ├── migration-review/
	│   └── security-review/
	└── commands/
```

- Root CLAUDE.md: architecture, stack, build/test commands, key boundaries
- Controller CLAUDE.md: request validation, DTOs, status codes, auth rules
- Service CLAUDE.md: business invariants, orchestration, domain exceptions
- Repository CLAUDE.md: parameterized queries, cancellation tokens, repository boundaries
- Migration CLAUDE.md: forward-only migrations, batching, rollback notes

### Rule of thumb

- CLAUDE.md = static guidance that explains the system and its boundaries
- Skills = reusable executable workflows such as migration review or API security review
- For large codebases, directory-scoped CLAUDE.md is usually better than one giant central rule file

### Example task

"Fix the auth token expiry bug. Read @src/controllers/auth.ts and @src/services/authService.ts, run the auth tests, add a regression test, and report the exact result."

This pattern matches Anthropic's current guidance: keep the project memory lean, and let Claude load only the instructions relevant to the directory it is working in.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Claude Code Memory (CLAUDE.md)](https://code.claude.com/docs/en/memory)
- [Context engineering](https://code.claude.com/docs/en/context-engineering)
- [Permission modes](https://code.claude.com/docs/en/permission-modes)

[← Previous slide](slide-05-what-this-session-covers.md) · [Deck index](../README.md)