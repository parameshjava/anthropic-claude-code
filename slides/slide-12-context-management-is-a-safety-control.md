# Slide 12: Context management is a safety control

### Context is finite working memory and a safety boundary

- The context window holds the task, conversation, file reads, command output, rules, skills, and connected-tool information. As irrelevant material grows, less room remains for the constraints Claude needs to reason correctly.
- A boundary stated only in chat can disappear after compaction. Put controls that must survive in `CLAUDE.md`, path-scoped rules, permission rules, hooks, CI, or pull-request policy.
- Context management is therefore not only about cost. It protects scope, safety constraints, and the quality of the next decision.

### Read the session state before it drifts

| State | What it looks like | Best response |
| --- | --- | --- |
| Healthy | Focused task, relevant files, clear responses | Continue with targeted reads and commands |
| Watch | Long logs, repeated file reads, growing unrelated history | Run `/context`; delegate large research; compact with guidance |
| Wrong path | The approach or edited code is no longer acceptable | Stop, use `/rewind`, then restart from the last sound checkpoint |
| New task | The current work is complete and the next request is unrelated | Run `/clear`, then begin with a clean prompt and appropriate effort |

### Use the right control at the right time

- **`/context`**: inspect what is loaded and what consumes the context budget before a large task or when answers lose precision.
- **`/compact <instructions>`**: use proactively at major milestones in the same task. Tell Claude exactly what to preserve: modified files, root cause, tests, unresolved decisions, and active safety boundaries.
- **`/rewind`**: restore conversation, code, or both after a wrong direction. Use it early rather than carrying failed approaches into later reasoning. Checkpoints do not replace Git for Bash-driven or external changes.
- **`/clear`**: discard an unrelated finished task. Clear is not compact: compact retains a summary for the same work; clear creates space for a new problem.

```text
/compact keep the auth root cause, modified files, regression test command,
and my instruction not to push until I review the diff
```

### Match effort to the task

- Use lower effort for a clear, local task such as a typo, a known one-line fix, or a narrow log classification.
- Use higher effort for ambiguous defects, cross-layer changes, migration planning, unfamiliar architecture, or security-sensitive reasoning.
- Change effort with `/effort` before changing models when the task needs more or less reasoning depth. Do not spend high-effort reasoning on a task whose expected diff is already obvious.

### Keep MCP and external tools lean

- Every MCP server introduces available tools, trust decisions, and potential context, latency, and output overhead. Tool search defers full tool schemas by default, but unused servers and large results still consume attention and tokens.
- Prefer an existing CLI tool when available; Anthropic identifies tools such as `gh`, `aws`, `gcloud`, and `sentry-cli` as more context-efficient than MCP. When a stable service API already meets the need, prefer a narrow API or CLI integration over a broad, general-purpose MCP server.
- Add MCP only when it removes repeated manual data transfer or enables a workflow a direct API or CLI cannot serve cleanly. Review server trust, scopes, and tool permissions; disable unused servers in `/mcp`.
- Keep tool results bounded. Ask for summaries, pagination, or filtered outputs instead of loading full logs, schemas, or issue histories into the main conversation.

### Example: finish one task, start another cleanly

After completing an authentication fix, run `/compact` with the modified files and test command if follow-up work remains. If the next task is a documentation update, run `/clear`, lower effort, reference the target files directly, and disable any unused monitoring or issue-tracker MCP servers first.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Permission modes](https://code.claude.com/docs/en/permission-modes)
- [Permissions](https://code.claude.com/docs/en/permissions)
- [Memory](https://code.claude.com/docs/en/memory)
- [Context window](https://code.claude.com/docs/en/context-window)
- [MCP integrations](https://code.claude.com/docs/en/mcp)
- [Cost and context management](https://code.claude.com/docs/en/costs)

[← Previous slide](slide-11-auto-mode-the-classifier.md) · [Deck index](../README.md)