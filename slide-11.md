# Chapter 11: Which Feature Solves Which Problem?

## Use the Lightest-Weight Mechanism That Still Gives Claude What It Needs

---

One of the most common questions from teams adopting Claude Code is: "Where do I put this configuration?" The following section maps five features to their ideal use cases so you always pick the right tool.

The principle is simple: **use the lightest-weight mechanism that solves your problem.**

### 1. CLAUDE.md — Persistent Project Memory

**Loads at:** Session start (always on)

**Best for:** Project context that every task needs — your tech stack, architecture overview, build commands, coding standards, and known gotchas.

**Example:**
```markdown
# ProjectName

## Tech Stack
TypeScript, Express, PostgreSQL, Redis

## Architecture
- /src/api/ — REST endpoints (one file per resource)
- /src/services/ — business logic (no direct DB imports)
- /src/db/ — repositories and migrations

## Key Commands
- npm test — vitest
- npm run lint — eslint + prettier
- npm run build — tsc

## Code Style and Conventions
- All API responses: { data, error, meta }
- Use Zod schemas for request validation
- Repository pattern for all database access

## Development Workflow
- Branch from main, PR required, CI must pass
```

**Rule of thumb:** If it applies to every task in this repo, it belongs in CLAUDE.md.

### 2. Rules — Scoped Guidance

**Loads:** Only for path-matched files

**Best for:** Conventions that apply to specific file types or directories — not globally.

**Example:** Create `.claude/rules/test-conventions.md`:
```markdown
---
globs: "**/*.test.ts"
---
Use vitest, mock external APIs, never use snapshots.
Test behavior, not implementation.
Each test file should mirror the source file it tests.
```

This rule only activates when Claude touches test files. Zero cost for all other tasks.

Another example — `.claude/rules/api-conventions.md`:
```markdown
---
globs: "src/api/**/*.ts"
---
Every endpoint must have Zod validation on request body.
Use the respondSuccess/respondError helpers from @src/api/utils.ts.
Always include OpenAPI JSDoc annotations.
```

**Rule of thumb:** If it applies to a specific directory or file pattern, use a rule.

### 3. Skills — Composable Expertise Packages

**Loads:** Via `/slash-command` or automatically when relevant

**Best for:** Repeatable workflows that involve multiple steps — doc generation, TDD, code review, design.

**Official skills include:**
- `frontend-design` — UI component design workflow
- `mcp-builder` — building MCP servers
- `skill-creator` — creating custom skills

**Community skills** (like `superpowers`, `remotion`, `webapp-testing`) add brainstorming, systematic debugging, parallel agent dispatch, and more.

**Example usage:**
```
/test-driven-development
Add a rate limiter to the /api/upload endpoint.
Max 10 requests per minute per API key.
```

The skill orchestrates: write failing test first, implement the feature, verify tests pass, refactor.

**Rule of thumb:** If it is a multi-step workflow you repeat across projects, make it a skill.

### 4. MCP — External Connections

**Loads:** Tool definitions at session start

**Best for:** Connecting to external systems — Slack, JIRA, databases, APIs, dashboards.

**Example:**
```
Read JIRA-1234, update status to "In Review",
post a summary to #eng-updates on Slack.
```

With MCP servers for JIRA and Slack configured, Claude executes this directly. No context switching, no copy-pasting between tools.

**Rule of thumb:** If it connects Claude to an external system, use MCP.

### 5. Hooks — Deterministic Lifecycle Triggers

**Loads:** On trigger events (not always-on)

**Best for:** Guarantees. Anything that MUST happen — linting, formatting, security gates, test runs.

**Examples:**
```json
{
  "PostToolUse": [{ "command": "eslint --fix $FILEPATH" }],
  "PreToolUse": [{ "command": "block-rm-rf.sh" }],
  "Stop": [{ "command": "curl -X POST https://api.internal/log" }]
}
```

**Critical distinction:** CLAUDE.md rules are **advisory** — Claude tries to follow them but might forget. Hooks are **guaranteed** — they execute deterministically every time.

**Rule of thumb:** If it must happen every time without exception, use a hook.

---

## Decision Matrix

| Question | Answer -> Feature |
|----------|-----------------|
| Does every task need this? | CLAUDE.md |
| Only for certain file types? | Rules |
| Multi-step repeatable workflow? | Skills |
| External system integration? | MCP |
| Must happen, no exceptions? | Hooks |
