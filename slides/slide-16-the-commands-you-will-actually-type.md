# Slide 16: The commands you will actually type

### The real power is in the operational controls

- These commands make the session observable and controllable. Use them at transitions: before a consequential change, after a wrong turn, and between unrelated tasks.

| Command | What it tells or changes | Reach for it when |
| --- | --- | --- |
| `/status` | Session, model, provider, and configuration state | You need to confirm where or how the session is running |
| `/context` | Loaded guidance, context usage, and optimization hints | Replies lose focus or a large task is about to begin |
| `/plan` or Plan mode | Explore and propose without source edits | Scope, architecture, migration impact, or risk is unclear |
| `/permissions` | Effective allow, ask, and deny rules; recent denials | A tool is blocked or a human checkpoint must be verified |
| `/compact <guidance>` | Summarizes the same task while retaining selected facts | The task continues but exploration and logs are crowding it |
| `/rewind` | Restores conversation, code, or both to a checkpoint | Claude took the wrong approach and you want a clean recovery |
| `/clear` | Starts a new task with an empty conversation context | The next request is unrelated to the completed work |
| `/mcp` | Reviews, connects, or disables external integrations | You need to check trust, authentication, or unused servers |

### A practical change sequence

```text
/status  →  /context  →  /plan  →  approve implementation
	→  /permissions (when an action is sensitive)  →  verify
	→  /compact (same task) or /clear (new task)
```

Example: before a schema migration, inspect `/status`, use `/context`, enter `/plan`, and check `/permissions` for the migration `ask` rule. After the implementation and tests, compact only if follow-up work remains; otherwise clear before starting unrelated documentation work.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Permission modes](https://code.claude.com/docs/en/permission-modes)
- [Permissions](https://code.claude.com/docs/en/permissions)
- [Memory](https://code.claude.com/docs/en/memory)
- [Commands](https://code.claude.com/docs/en/commands)

[← Previous slide](slide-15-a-budget-is-not-a-boundary.md) · [Deck index](../README.md)