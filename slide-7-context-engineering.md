# Chapter 7: Context Engineering — What Should Load When

## More Context Is Not Automatically Better — Load Only What the Task Needs, and Force Specific File Paths

---

One of the most misunderstood aspects of Claude Code is the assumption that "more context = better results." That is wrong. **Targeted context = better results.** Loading everything dilutes signal with noise and burns the context window.

There are three tiers of context loading.

### Tier 1: Always On — Loads at Session Start

These are the **persistent foundations**:

**Project CLAUDE.md** sits in the repository root. It contains architecture decisions, coding standards, team conventions, build commands, and project-specific gotchas. Every Claude Code session reads this first.

Example CLAUDE.md content:
```markdown
# MyApp

## Tech Stack
- TypeScript, Node.js, PostgreSQL, Redis
- Framework: Express with Zod validation

## Key Commands
- npm test — runs vitest
- npm run lint — eslint + prettier
- npm run migrate — runs database migrations

## Architecture
- /src/api/ — REST endpoints
- /src/db/ — database layer, migrations
- /src/services/ — business logic

## Conventions
- All API responses use { data, error, meta } shape
- Never import from /src/db/ directly in /src/api/ — go through /src/services/
```

**User / org CLAUDE.md** lives in `~/.claude/CLAUDE.md` and applies across all projects. Use it for personal preferences or org-wide standards.

**Unscoped rules** fire on every task regardless of file path. Use them for universal policies like "always run lint after editing TypeScript files."

**Auto memory** loads timestamped notes from previous sessions — preferences, decisions, and feedback.

### Tier 2: On Demand — Load Only When Needed

**Path-scoped rules** activate only when Claude touches files in specific directories. For example:

```
# .claude/rules/test-conventions.md
---
globs: "**/*.test.ts"
---
Use vitest. Mock external APIs. Never use snapshot tests.
Assert behavior, not implementation details.
```

This rule only loads when Claude works on test files — zero cost for all other tasks.

**Skills** are reusable prompt templates loaded dynamically via slash commands or automatically when relevant. The official skill library includes `frontend-design`, `mcp-builder`, `test-driven-development`, and more. Community skills like `superpowers` add brainstorming, systematic debugging, and code review workflows.

**@ file references** are the most powerful context tool. Instead of saying "fix the auth bug," write:

```
Fix the token expiry bug in @src/auth/handler.ts.
Check the token generation logic in @src/auth/token.ts.
```

This forces Claude to load exactly those two files — no scanning, no guessing.

**Subagent prompts** return clean summaries from isolated investigations. The noisy details stay in the subagent's context, and the main session gets a concise result.

### Tier 3: External + Automated

**MCP servers** connect to databases, APIs, dashboards. Configured via `.mcp.json` and checked into the repo.

**Hook scripts** fire at lifecycle events — auto-lint after edits, security gates before tool execution, CI triggers on commit.

**GitHub Actions** automate code review, PR descriptions, and test generation in the CI/CD pipeline.

### Essential Context Management Commands

These are the commands every engineer should use daily for managing context:

| Command                                  | What It Does                                                                                             | When to Use                                                                                    |
| ---------------------------------------- | -------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| `/clear`                                 | Resets context completely (aliases: `/reset`, `/new`)                                                    | Between unrelated tasks — e.g., switching from an auth bug to a UI feature                     |
| `/compact [instructions]`                | Trims conversation history while preserving key facts about modified files. Optional focus instructions. | Long sessions — e.g., `/compact keep the auth handler changes and test results`                |
| `/context`                               | Visualizes current context usage as a colored grid with optimization suggestions                         | Before starting a large task — check if idle MCP servers or stale context are consuming budget |
| `/effort [low\|medium\|high\|max\|auto]` | Sets reasoning depth. `low`/`medium`/`high` persist across sessions. `max` is session-only (Opus 4.6).   | `/effort low` for a typo fix, `/effort high` for a complex refactor                            |

### Additional Useful Commands

| Command                | What It Does                                                                   | Example                                                                                                      |
| ---------------------- | ------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------ |
| `/plan`                | Enters plan mode — Claude thinks and proposes an approach but does not execute | `/plan` before refactoring the notification service across 8 files                                           |
| `/model [model]`       | Switch the active model mid-session                                            | `/model opus` when a Sonnet debugging session hits a deep cross-cutting issue                                |
| `/cost`                | Shows token usage statistics for the current session                           | Check after a long investigation to see how much context was consumed                                        |
| `/diff`                | Opens interactive diff viewer showing uncommitted changes and per-turn diffs   | Review all changes before committing — see exactly what Claude modified                                      |
| `/mcp`                 | Manage MCP server connections — enable, disable, or authenticate               | `/mcp` to disconnect idle Slack/Jira servers that consume context without being used                         |
| `/memory`              | Edit CLAUDE.md memory files, enable/disable auto-memory                        | `/memory` to add a permanent rule like "never scan vendor/"                                                  |
| `/permissions`         | View or update tool permission rules (alias: `/allowed-tools`)                 | `/permissions` to allow Bash commands for npm without repeated prompts                                       |
| `/init`                | Initialize project with a CLAUDE.md guide                                      | `/init` on a new repo to scaffold the initial CLAUDE.md from project structure                               |
| `/resume [session]`    | Resume a previous conversation by ID or name (alias: `/continue`)              | `/resume auth-bug-fix` to pick up yesterday's debugging session                                              |
| `/rewind`              | Rewind conversation and/or code to a previous point (alias: `/checkpoint`)     | `/rewind` when Claude went down a wrong path — undo the last 3 turns and try a different approach            |
| `/batch <instruction>` | Orchestrate large-scale parallel changes across the codebase                   | `/batch add OpenAPI annotations to all endpoints in src/api/` — spawns agents per file in isolated worktrees |
| `/export [filename]`   | Export the current conversation as plain text                                  | `/export auth-investigation.txt` to share a debugging session with a teammate                                |
| `/security-review`     | Analyze pending changes for security vulnerabilities                           | `/security-review` before committing changes to authentication or payment code                               |

### Real-World Example: Context Commands in a Typical Session

```
Morning — starting fresh:
  You: /init                          → scaffolds CLAUDE.md for the new project
  You: /effort high                   → complex feature work ahead, need deep reasoning

Working on auth refactor:
  You: /plan                          → preview approach before touching 6 files
  You: [implement the auth changes]
  You: /diff                          → review all changes Claude made
  You: /security-review               → scan auth changes for vulnerabilities
  You: /cost                          → check: used 45K tokens so far

Switching to an unrelated UI bug:
  You: /clear                         → wipe auth context, start fresh
  You: /effort low                    → simple CSS fix, no deep reasoning needed
  You: [fix the UI bug]

End of day — complex investigation:
  You: /effort high                   → back to deep reasoning
  You: /context                       → check: 3 idle MCP servers eating context
  You: /mcp                           → disconnect Jira and Figma servers
  You: [run the investigation]
  You: /compact keep the root cause analysis and proposed fix
                                      → free up context while preserving findings
  You: /export perf-investigation.txt → save the session for tomorrow
```

### Permissions: Where They Are Stored and How to Configure

Permissions control which tools Claude can use without prompting. They are stored in JSON settings files at three levels:

| Scope          | File Path                                       | Applies To                            |
| -------------- | ----------------------------------------------- | ------------------------------------- |
| **Project**    | `.claude/settings.json` (repo root)             | Everyone on the team (commit to git)  |
| **User**       | `~/.claude/settings.json`                       | All projects for this user            |
| **Enterprise** | Managed via plist (macOS) or Registry (Windows) | All users in the org (pushed via MDM) |

**Adding allow/deny rules:**

Edit the appropriate settings file and add an `allowedTools` or `deniedTools` array:

```json
{
  "permissions": {
    "allowedTools": [
      "Bash(npm test)",
      "Bash(npm run lint)",
      "Bash(npx prettier --write *)",
      "Edit",
      "Write",
      "Read",
      "Glob",
      "Grep"
    ],
    "deniedTools": [
      "Bash(rm -rf *)",
      "Bash(DROP TABLE *)",
      "Bash(curl * | bash)"
    ]
  }
}
```

**Permission rule patterns:**

| Pattern                      | What It Allows/Denies                     |
| ---------------------------- | ----------------------------------------- |
| `"Bash(npm test)"`           | Exact command: only `npm test`            |
| `"Bash(npm *)"`              | Wildcard: any command starting with `npm` |
| `"Edit"`                     | All file edits without prompting          |
| `"mcp__github__*"`           | All tools from the GitHub MCP server      |
| `"mcp__slack__post_message"` | Only the post_message tool from Slack MCP |

**Real-world example — a team's `.claude/settings.json`:**

```json
{
  "permissions": {
    "allowedTools": [
      "Bash(npm test *)",
      "Bash(npm run lint *)",
      "Bash(npx vitest *)",
      "Bash(git status)",
      "Bash(git diff *)",
      "Bash(git log *)",
      "Edit",
      "Write",
      "Read",
      "Glob",
      "Grep",
      "mcp__github__*"
    ],
    "deniedTools": [
      "Bash(rm -rf *)",
      "Bash(git push --force *)",
      "Bash(git reset --hard *)",
      "Bash(DROP *)",
      "Bash(curl * | bash)"
    ]
  }
}
```

This configuration lets Claude run tests, lint, and read/write files freely, while blocking destructive commands like `rm -rf`, force pushes, and piped shell execution. Commit this to the repo so every team member gets the same safety guardrails automatically.

**Tip:** Use `/permissions` in Claude Code to interactively review and modify current permission rules without editing JSON directly.

### CLI Terminal Commands and Flags

These are flags passed when launching `claude` from the terminal — they control how the session starts, what model to use, permissions, output format, and more.

#### Session Modes

| Command                                          | Description                                                                                       |
| ------------------------------------------------ | ------------------------------------------------------------------------------------------------- |
| `claude`                                         | Start an interactive REPL session                                                                 |
| `claude -p "prompt"` / `claude --print "prompt"` | Non-interactive mode — prints response and exits. Essential for CI/CD pipelines and shell scripts |
| `claude -c` / `claude --continue`                | Continue the most recent conversation in the current directory                                    |
| `claude -r [session-id]` / `claude --resume`     | Resume a specific session by ID, or open a session picker                                         |
| `claude -w [name]` / `claude --worktree`         | Create a new git worktree for isolated parallel work                                              |

```bash
# Start a new interactive session
claude

# Run a one-shot prompt in CI and exit
claude -p "Summarize the changes in the last 5 commits"

# Continue where you left off yesterday
claude -c

# Resume a named session from last week
claude -r auth-refactor-session

# Work on a feature in an isolated worktree
claude -w feature-rate-limiter
```

#### Model and Effort

| Flag                       | Description                                                                           |
| -------------------------- | ------------------------------------------------------------------------------------- |
| `--model <model>`          | Override model for this session — e.g., `--model opus` or `--model claude-sonnet-4-6` |
| `--fallback-model <model>` | Automatic fallback when primary model is overloaded (only with `--print`)             |
| `--effort <level>`         | Set reasoning effort: `low`, `medium`, `high`, `max`                                  |

```bash
# Use Opus for a complex architecture task
claude --model opus

# Use Sonnet with Haiku as fallback in CI (if Sonnet is rate-limited)
claude -p --model sonnet --fallback-model haiku "Generate test coverage report"

# Low effort for a simple rename
claude --effort low

# Max effort for a critical security audit
claude --effort max --model opus
```

#### Permissions and Security

| Flag                             | Description                                                                                                                  |
| -------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| `--dangerously-skip-permissions` | Bypass ALL permission checks. **Only use in sandboxed environments with no internet access** — e.g., Docker containers in CI |
| `--permission-mode <mode>`       | Fine-grained control: `default`, `acceptEdits` (auto-approve edits), `plan` (read-only), `bypassPermissions`                 |
| `--allowed-tools <tools...>`     | Whitelist specific tools — e.g., `"Bash(git:*) Edit Read"`                                                                   |
| `--disallowed-tools <tools...>`  | Blacklist specific tools — e.g., `"Bash(rm *) Bash(curl * \| bash)"`                                                         |

```bash
# CI pipeline in a sandboxed Docker container — skip all permission prompts
claude -p --dangerously-skip-permissions "Run the full test suite and report failures"

# Read-only analysis — Claude can explore but not modify anything
claude --permission-mode plan

# Auto-approve file edits but still prompt for shell commands
claude --permission-mode acceptEdits

# Allow only git and test commands, block everything destructive
claude --allowed-tools "Bash(git *) Bash(npm test *) Read Grep Glob Edit" \
       --disallowed-tools "Bash(rm *) Bash(curl * | bash) Bash(git push --force *)"

# Junior developer mode — read and search only, no edits or shell access
claude --allowed-tools "Read Grep Glob" --disallowed-tools "Edit Write Bash"
```

#### Output Control (for `--print` mode)

| Flag                       | Description                                                                               |
| -------------------------- | ----------------------------------------------------------------------------------------- |
| `--output-format <format>` | `text` (default), `json` (single JSON result), `stream-json` (real-time streaming NDJSON) |
| `--json-schema <schema>`   | Enforce structured output with a JSON Schema — useful for automation pipelines            |
| `--verbose`                | Show additional detail in output                                                          |

```bash
# Plain text output (default)
claude -p "Explain the auth flow in src/auth/"

# JSON output for parsing in a script
claude -p --output-format json "List all TODO comments in the codebase"

# Streaming JSON for real-time progress in a dashboard
claude -p --output-format stream-json "Refactor the billing module"

# Structured output — force response to match a schema
claude -p --json-schema '{
  "type": "object",
  "properties": {
    "bugs": {"type": "array", "items": {"type": "string"}},
    "severity": {"type": "string", "enum": ["low","medium","high","critical"]}
  },
  "required": ["bugs", "severity"]
}' "Find bugs in src/api/payments.ts"

# Verbose output for debugging Claude's behavior
claude -p --verbose "Why is the test in src/auth/token.test.ts failing?"
```

#### Budget and Limits

| Flag                        | Description                                                            |
| --------------------------- | ---------------------------------------------------------------------- |
| `--max-budget-usd <amount>` | Hard spending cap in USD for the session. Critical for CI cost control |

```bash
# Cap a CI job at $2 — session stops if budget is exhausted
claude -p --max-budget-usd 2.00 "Review this PR for security issues"

# Higher budget for a complex multi-file refactor
claude -p --max-budget-usd 10.00 --model opus "Refactor the monolith auth module into microservices"
```

#### Prompt Customization

| Flag                              | Description                                                                                        |
| --------------------------------- | -------------------------------------------------------------------------------------------------- |
| `--system-prompt <prompt>`        | **Replace** the default system prompt entirely — for creating specialized agents                   |
| `--append-system-prompt <prompt>` | **Append** to the default system prompt without replacing it — safer for adding extra instructions |

```bash
# Create a specialized security reviewer agent
claude --system-prompt "You are a security-focused code reviewer. \
  Only report security vulnerabilities. Ignore style issues. \
  Rate each finding as critical, high, medium, or low."

# Add team-specific instructions without replacing the default prompt
claude --append-system-prompt "Always follow our team's API conventions: \
  use Zod validation, return {data, error, meta} shape, \
  and include OpenAPI annotations on every endpoint."

# Combine with --print for a custom CI agent
claude -p --append-system-prompt "You are a test generator. \
  Write vitest tests only. Mock external APIs. \
  Never use snapshot tests." \
  "Generate tests for src/api/billing.ts"
```

#### MCP Configuration

| Flag                  | Description                                                                                                          |
| --------------------- | -------------------------------------------------------------------------------------------------------------------- |
| `--mcp-config <path>` | Load MCP server configs from a JSON file                                                                             |
| `--strict-mcp-config` | **Only** use MCP servers from `--mcp-config`, ignoring all other MCP configs. Important for locked-down environments |

```bash
# Load team MCP config from a shared file
claude --mcp-config ./team-mcp.json

# Locked-down environment — only use approved MCP servers, ignore user/global configs
claude --mcp-config ./approved-servers.json --strict-mcp-config

# Combine with permission controls for a fully controlled CI environment
claude -p --mcp-config ./ci-mcp.json --strict-mcp-config \
  --allowed-tools "Read Grep mcp__github__*" \
  --max-budget-usd 3.00 \
  "Analyze the open PRs and summarize review status"
```

#### Subcommands

| Command                                    | Description                                                                                                     |
| ------------------------------------------ | --------------------------------------------------------------------------------------------------------------- |
| `claude mcp add <name> <cmd>`              | Add an MCP server — e.g., `claude mcp add github -e GITHUB_TOKEN -- npx -y @modelcontextprotocol/server-github` |
| `claude mcp list`                          | List all configured MCP servers                                                                                 |
| `claude mcp remove <name>`                 | Remove an MCP server                                                                                            |
| `claude auth login` / `claude auth logout` | Sign in or sign out of your Anthropic account                                                                   |
| `claude update`                            | Check for and install updates                                                                                   |
| `claude doctor`                            | Diagnose installation and configuration issues                                                                  |

```bash
# Add GitHub MCP server scoped to this project (saved in .mcp.json)
claude mcp add github --scope project -e GITHUB_TOKEN \
  -- npx -y @modelcontextprotocol/server-github

# Add Postgres MCP server for the current user (all projects)
claude mcp add postgres --scope user -e DATABASE_URL \
  -- npx -y @modelcontextprotocol/server-postgres

# Add Slack MCP server with OAuth authentication
claude mcp add slack --scope project \
  -- npx -y @anthropic/mcp-server-slack

# List all configured servers and their scopes
claude mcp list

# Remove an MCP server you no longer need
claude mcp remove slack

# Sign in to your Anthropic account
claude auth login

# Check authentication status
claude auth status

# Diagnose installation issues (version, auth, connectivity)
claude doctor

# Update Claude Code to the latest version
claude update
```

#### Real-World Enterprise Examples

**CI/CD pipeline — automated PR review with cost cap:**
```bash
claude -p --model opus --max-budget-usd 5.00 \
  --output-format json \
  --dangerously-skip-permissions \
  "Review this PR for security issues and return findings as JSON"
```

**Read-only analysis — no file modifications allowed:**
```bash
claude -p --permission-mode plan \
  --allowed-tools "Read Grep Glob" \
  "Analyze the authentication flow in src/auth/ and identify potential issues"
```

**Structured output for automation pipelines:**
```bash
claude -p --json-schema '{"type":"object","properties":{"bugs":{"type":"array"},"severity":{"type":"string"}},"required":["bugs"]}' \
  "Find bugs in src/api/"
```

**Locked-down environment with controlled MCP servers:**
```bash
claude --mcp-config ./team-mcp.json --strict-mcp-config \
  --allowed-tools "Read Edit Bash(npm test *) mcp__github__*" \
  --disallowed-tools "Bash(rm *) Bash(curl * | bash)"
```

**Parallel feature work in isolated worktree:**
```bash
claude -w feature-auth --model opus \
  "Implement the SSO integration described in JIRA-4567"
```

---

## Key Takeaway

Context engineering is like database indexing — the goal is not to load everything, but to load exactly what is needed, exactly when it is needed. Train your teams to think in terms of **always-on, on-demand, and external** tiers, and use these commands to keep context lean throughout the day.
