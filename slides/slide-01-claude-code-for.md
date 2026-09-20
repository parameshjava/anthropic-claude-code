# Slide 1: Claude Code for

### What Claude Code is

Claude Code is Anthropic's coding agent for software engineering work. It is a command-line and IDE-style assistant that can read a codebase, reason about the repository, make code changes, run commands, and validate results with the engineer in the loop.

### What it can do

- Understand the project structure and existing code patterns
- Search, inspect, and explain files across a repo
- Edit code, create new files, and refactor implementation
- Run tests, linters, builds, and other verification commands
- Follow project conventions, guardrails, and permission rules
- Help with debugging, implementation, migration, and maintenance tasks
- Work in a structured, tool-driven loop instead of only generating text

### Why it matters

It turns AI from a code generator into a practical engineering collaborator: one that can operate on real repositories, use real tools, and produce verifiable changes.

### The useful mental model

Treat Claude Code like a highly capable senior engineer joining the team. It can reason and act, but it still needs onboarding: architecture, conventions, relevant tools, testing expectations, and boundaries.

| Team input | What it gives the agent |
| --- | --- |
| Context | The code, architecture, and decisions relevant to the task |
| Prompting | A clear outcome, scope, constraints, and proof of done |
| Delegation | Isolated research and review without polluting the main session |
| MCP and tools | Controlled access to the systems needed to do the work |

The better this onboarding is, the more safely and effectively Claude Code can contribute. Enterprise adoption is deliberate context and workflow design, not plug-and-play automation.


### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Permission modes](https://code.claude.com/docs/en/permission-modes)
- [Permissions](https://code.claude.com/docs/en/permissions)
- [Memory](https://code.claude.com/docs/en/memory)

[Deck index](../README.md)