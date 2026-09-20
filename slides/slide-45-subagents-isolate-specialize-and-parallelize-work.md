# Slide 45: Subagents: isolate, specialize, and parallelize work

### Subagents are useful when a task needs a separate context or role

- Subagents run in separate context windows. Use them to keep broad exploration, verbose output, or specialized review from crowding the main implementation session.
- Give every delegated task an output contract: scope, allowed tools, evidence required, exclusions, and the format of the returned result.

| Good delegation | Poor delegation |
| --- | --- |
| "Trace token refresh in `src/auth/`; return files, call flow, and open risks. Do not edit." | "Investigate auth." |
| "Review this diff for authorization gaps; report only correctness findings with file references." | "Check whether this code is good." |
| "Run the full suite and summarize failures, command, duration, and affected modules." | "Run tests." |

### Orchestrator pattern

```text
Main session: owns goal, plan, trade-offs, and final patch
	↓
Subagent: explores, tests, or reviews in isolated context
	↓
Main session: evaluates summary, applies decisions, verifies final result
```

- Parallelize independent work. Do not delegate two agents to edit the same files without isolation or coordination.
- Use a fresh reviewer subagent to challenge an implementation, but keep accountable human approval in the main engineering workflow.

### Example

Delegate a read-only dependency search to return file-and-line findings, then keep the design decision, code change, and final regression test in the main session.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Subagents](https://code.claude.com/docs/en/sub-agents)
- [Agent teams](https://code.claude.com/docs/en/agent-teams)

[← Previous slide](slide-44-which-feature-solves-which-problem.md) · [Deck index](../README.md)