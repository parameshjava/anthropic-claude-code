# Chapter 16: Hooks — Automate Every Step of the Agent Lifecycle

## Deterministic Control That Is Guaranteed to Execute

---

Hooks are arguably the most important feature for enterprise adoption, because they solve a fundamental problem: **CLAUDE.md rules are advisory. Hooks are guaranteed.**

Placing "always run lint after editing" in CLAUDE.md means Claude *tries* to follow it. But under context pressure, or in complex multi-step tasks, it might forget. Placing the same rule in a hook means it executes deterministically — every time, no exceptions.

The difference between "probably" and "always" is everything in enterprise engineering.

### The Six Lifecycle Events

Hooks fire at specific points in the agent lifecycle:

**1. SessionStart** — fires when a Claude Code session begins.
Use it to: load environment variables, set up context, initialize connections.
```json
{ "event": "SessionStart", "type": "command", "command": "source .env.local" }
```

**2. PromptSubmit** — fires when the user submits a prompt.
Use it to: validate input, transform prompts, add standard context.
```json
{ "event": "PromptSubmit", "type": "prompt", "prompt": "Does this prompt contain production credentials? If yes, block it." }
```

**3. PreToolUse** — fires before Claude executes any tool.
Use it to: approve or block actions. **Exit code 2 = blocked.** This is the security gate.
```json
{ "event": "PreToolUse", "type": "command", "command": "check-dangerous-command.sh" }
```

Example: block any `rm -rf` or `DROP TABLE` command before it ever executes.

**4. PostToolUse** — fires after Claude executes a tool.
Use it to: format code, run linters, execute tests.
```json
{ "event": "PostToolUse", "matcher": "Edit|Write", "type": "command", "command": "npx prettier --write $FILEPATH && npx eslint --fix $FILEPATH" }
```

Every file Claude edits is automatically formatted and linted. Every time.

**5. Stop** — fires when Claude tries to end the task.
Use it to: enforce task completion. **Exit code 2 = forced continue.** Claude cannot stop until the conditions are met.
```json
{ "event": "Stop", "type": "prompt", "prompt": "Has the agent run all tests and confirmed they pass? If not, return exit 2." }
```

This prevents Claude from declaring completion without actually verifying its work.

**6. PreCompact** — fires before context compaction.
Use it to: backup conversation state, save important context.
```json
{ "event": "PreCompact", "type": "command", "command": "save-conversation-snapshot.sh" }
```

### The Four Handler Types

**`type: "command"`** — Shell scripts via stdin/stdout. The most common type. Claude's event data is piped to stdin, and the script's exit code determines the outcome.
```
npx prettier --write $FILEPATH
```

**`type: "http"`** — POST event JSON to a URL endpoint. Ideal for connecting to internal policy servers, logging systems, or approval workflows.
```
http://policy-server:8080/validate
```

**`type: "prompt"`** — Single-turn LLM evaluation. Claude evaluates a question about the current action. Useful for nuanced checks that a shell script cannot handle.
```
Does this edit touch authentication or authorization logic?
If yes, require a security review flag.
```

**`type: "agent"`** — Multi-turn sub-agent verification. A full agent is spawned to verify the action. Use for complex validation like "verify test coverage for all changed functions."

### Enterprise Pattern: Why Hooks Over CLAUDE.md?

| CLAUDE.md | Hooks |
|-----------|-------|
| "Please run lint after editing" | Lint runs automatically after every edit |
| "Don't delete production data" | `rm` on production paths is blocked with exit 2 |
| "Run tests before saying done" | Stop hook forces test execution before completion |
| Advisory — best effort | Guaranteed — deterministic |

### Configuration Scope

**Project-level:** `.claude/settings.json` — shared via git, applies to everyone on the team.

**User-level:** `~/.claude/settings.json` — personal defaults, applies to all projects.

Project-level hooks override user-level for the same event, so team security gates always apply.

### Critical Enterprise Detail

> Hooks fire recursively for subagent actions — safety gates apply to all spawned agents automatically.

If a PreToolUse hook blocks `rm -rf`, it blocks it for the main agent AND every subagent it spawns. There is no way for a subagent to bypass the security gate. This is essential for enterprise trust.

---

## Example: Complete Hook Configuration

```json
{
  "hooks": {
    "SessionStart": [{
      "type": "command",
      "command": "echo 'Session $(date) by $(whoami)' >> .claude/audit.log"
    }],
    "PreToolUse": [{
      "matcher": "Bash",
      "type": "command",
      "command": "check-safe-command.sh"
    }],
    "PostToolUse": [{
      "matcher": "Edit|Write",
      "type": "command",
      "command": "npx prettier --write $FILEPATH"
    }],
    "Stop": [{
      "type": "prompt",
      "prompt": "Verify: all tests pass, no lint errors, changed files listed. If not, exit 2."
    }]
  }
}
```

This configuration logs every session, blocks unsafe commands, auto-formats every edit, and prevents completion without verification. All deterministic. All guaranteed.
