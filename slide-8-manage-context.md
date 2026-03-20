# Chapter 8: Manage Context — Avoid Limits, Force Specificity

## Stop Claude from Scanning the Entire Project — Tell It Exactly Where to Look

---

The single most important operational habit is this: **tell Claude exactly where to look, and tell it where NOT to look.**

### The Context Budget

The context window is a budget — a finite amount where every token spent on irrelevant content is a token not available for reasoning about the actual problem.

There are three zones to be aware of:
- **Healthy** — well within limits, Claude has room to reason
- **Watch** — getting full, responses may lose detail
- **Compact / Clear** — at the limit, Claude will start forgetting earlier parts of the conversation

### Four Tactics to Stay in the Green

#### 1. Use @ File References

This is the single most impactful habit. Compare these two prompts:

**Bad:** "Fix the authentication bug"
- Claude scans the entire project looking for auth-related files
- It reads files it does not need, consuming context
- It might modify files you did not intend
- No clarity on what is broken, what standard to follow, or what success looks like

**Good:**
```
JWT token claims are not aligned with RFC-7519 section 4.1,
causing 401 errors for valid users after session refresh.

Read @src/auth/handler.ts and @src/auth/token.ts.
The "iss" and "aud" claims are missing from the token payload.
Fix: add the required registered claims per RFC-7519 section 4.1.
Expected outcome: existing auth tests pass, session refresh
returns 200 with valid claims. Add a regression test for missing claims.
Do not change the token format or middleware signature.
```
- Claude loads exactly two files — zero scanning, zero wasted context
- Knows the RFC standard to follow and which section to reference
- Understands the current problem (missing claims causing 401 on refresh)
- Has a clear expected outcome (200 with valid claims, tests pass)
- Constraints prevent unintended scope creep

#### 2. Scope in CLAUDE.md

Add permanent navigation rules to CLAUDE.md:

```markdown
## Navigation Rules
- Read @src/auth/ before any authentication task
- Read @src/db/migrations/ before any database task
- Never scan node_modules/, dist/, or .next/
- Never scan files larger than 1000 lines without explicit instruction
```

These directives train Claude to navigate the codebase efficiently in every session. They act as permanent "GPS directions" so Claude never wanders.

#### 3. Compact + /effort + /clear

**`/compact`** trims conversation history while keeping critical information about modified files. Use it when a session gets long but work continues on the same task.

```
# Basic compact — Claude decides what to keep
You: /compact

# Guided compact — specify what matters
You: /compact keep the auth handler changes, test results, and root cause analysis

# After a long debugging session with lots of exploratory output
You: /compact preserve only the final fix in src/api/search.ts and the failing test output
```

**`/effort`** sets reasoning depth. For simple tasks like renaming a variable or fixing a typo, set effort to low — Claude spends fewer tokens reasoning and responds faster.

```
# Simple rename or typo fix — no deep reasoning needed
You: /effort low
You: Rename getUserData to fetchUserProfile in @src/services/user.ts

# Standard feature work — default reasoning
You: /effort medium
You: Add pagination to the /api/products endpoint

# Complex multi-file refactor — deep reasoning needed
You: /effort high
You: Refactor the payment processing pipeline to support multiple providers

# Maximum reasoning (Opus 4.6, session-only) — architecture decisions
You: /effort max
You: Redesign the event-driven notification system to support 3 new channels

# Reset to default behavior
You: /effort auto
```

**`/clear`** resets everything. Use it between completely unrelated tasks. There is no point keeping the context from a "fix auth bug" session when switching to "update the README."

```
# Done with auth bug, now working on a completely different feature
You: /clear
You: Add a CSV export endpoint for the /api/reports resource

# After a long investigation that consumed a lot of context
You: /clear
You: /effort low
You: Fix the typo in the error message in @src/middleware/errorHandler.ts
```

#### 4. Subagents + Disconnect MCP

**Subagents** are noise isolation chambers. Logs, test output, documentation generation — anything that produces large, verbose output — should run in a subagent. The subagent processes it in its own isolated context and returns a clean summary to the main session.

**MCP disconnection** is equally important. Every connected MCP server loads its tool schemas into context — even if it is never used. If 10 MCP servers are connected but only GitHub and Slack are needed for the current task, disconnect the other 8 with `/mcp`.

### Enterprise Tip: Sparse Checkout for Monorepos

For large monorepos, `worktree.sparsePaths` tells Claude to check out only the directories relevant to the task via git sparse-checkout. If the monorepo has 5-10 modules but work is focused on the one module (billing service), Claude only sees the billing directory — not all 5-10 modules.

**Where to add it depends on who needs it:**

**Option A: Personal settings (recommended for individual work)**
Use `~/.claude/settings.json` when different developers work on different services. Each developer configures their own sparse paths — no commits needed, no conflicts with teammates.

```json
// ~/.claude/settings.json (per-developer, NOT committed)
{
  "worktree": {
    "sparsePaths": ["services/billing/", "libs/shared/", "configs/"]
  }
}
```

When you switch to a different service, just update your personal settings:

```json
// Switching to the payments service — just edit your local file
{
  "worktree": {
    "sparsePaths": ["services/payments/", "libs/shared/", "configs/"]
  }
}
```

No commit, no PR, no impact on other developers.

**Option B: Project settings (for shared paths the whole team uses)**
Use `.claude/settings.json` (in the repo root, committed) only for paths that **every** developer on the project needs — like shared libraries or configs:

```json
// .claude/settings.json (committed — affects everyone)
{
  "permissions": {
    "allow": ["Bash(npm test)", "Bash(npm run build)"]
  },
  "worktree": {
    "sparsePaths": ["libs/shared/", "configs/"]
  }
}
```

**How they combine:** Personal settings merge with project settings. If the project config includes `libs/shared/` and your personal config adds `services/billing/`, Claude sees both. This way the team commits shared paths, and each developer adds their service-specific paths locally.

---

## Practical Exercise for Teams

Have each engineer run `/context` in their next Claude Code session. Examine what is loaded and ask:
1. Is everything loaded actually needed for this task?
2. What could be moved to on-demand loading?
3. Are there MCP servers consuming context that are not being used?

This five-minute exercise typically reveals 30-50% context savings.

---

## Useful Links

- [Effective Context Engineering for AI Agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- [Claude Code Memory (CLAUDE.md)](https://code.claude.com/docs/en/memory)
- [Claude Code Best Practices](https://code.claude.com/docs/en/best-practices)
- [All Claude Models Overview (Context Windows)](https://platform.claude.com/docs/en/about-claude/models/all-models)
