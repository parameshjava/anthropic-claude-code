# Chapter 15: Scaling Claude Code Across Teams

## Standardize Inputs So Claude Code Scales Beyond One Person

---

One engineer using Claude Code effectively is great. Fifty engineers using it consistently is transformational. This chapter provides a **six-step rollout playbook** for taking Claude Code from a single-user tool to an engineering-wide multiplier.

### Step 01: Lean CLAUDE.md

Start here. Every repository gets a CLAUDE.md file with:
- **Build commands** — how to build, test, lint, and deploy
- **Architecture overview** — directory structure and layer responsibilities
- **File scope rules** — "read @src/auth/ for auth tasks"
- **"Do not scan" directives** — "never scan node_modules, dist, .next, or vendor"

Keep it lean. CLAUDE.md loads at every session start, so every line costs context. Target under 100 lines. If more detail is needed, move it to rules or skills.

**Example rollout action:** Create a CLAUDE.md template for the organization. Every new repo starts with this template, customized to the project.

### Step 02: Scoped Rules + Skills

Once the CLAUDE.md is solid, move **bulky, conditional guidance** into rules and skills.

Rules activate only when needed (path-scoped). Skills load only when invoked. This keeps always-on context under **2% of the context window** — a critical threshold for maintaining Claude's reasoning quality.

**Example rollout action:** Identify the top 5 coding conventions that apply to specific file types. Create a rule for each:
- Test files → testing conventions
- API files → endpoint conventions
- Migration files → migration conventions
- Config files → security conventions
- CI files → pipeline conventions

### Step 03: Subagents + Teams

Set up **purpose-built agents** for repeatable tasks:
- **Test runner agent** — runs suites, summarizes failures
- **Log analysis agent** — scans logs for patterns
- **Code review agent** — reviews PRs against standards

Use **Agent Teams** for cross-module work — when a change touches the API, the database, and the frontend simultaneously.

**Example rollout action:** Create `.claude/agents/test-runner.md` and `.claude/agents/reviewer.md` in the repo. Share them with the team.

### Step 04: Team-Wide MCP Config

Check `.mcp.json` into the repository. Only connect MCP servers for **active workflows** — each idle server consumes context for its tool schemas.

**Example rollout action:** Audit the team's tool stack. Which tools do engineers context-switch to most? Start with those MCP integrations:
- GitHub (PRs, issues)
- Your issue tracker (JIRA, Linear)
- Your communication tool (Slack)
- Your database (Postgres, MongoDB)

### Step 05: Lifecycle Hooks

Hooks are the **guardrails at scale**. They guarantee that certain actions always happen, regardless of whether the individual engineer remembered to ask for them.

Essential hooks for every team:
- **PostToolUse → auto-lint** after every edit
- **PostToolUse → run relevant tests** after code changes
- **PreToolUse → security scan** before destructive operations
- **PreCompact → backup** before context compaction

**Example rollout action:** Add a `.claude/settings.json` with the team's hooks. Commit it to the repo.

### Step 06: Onboard Engineers

The final step is human. Every engineer needs:
- **The prompt formula** — goal, files, constraints, scope, validation, output
- **Context budget awareness** — how to use `/context`, when to `/compact`, when to `/clear`
- **The trust/verification loop** — never trust, always verify with tests and evidence

**Example rollout action:** Run a 30-minute workshop. Have each engineer:
1. Write a prompt using the formula for a real task
2. Check their context budget with `/context`
3. Set up one hook in their project

### The Bottom Line

> Claude Code becomes an engineering multiplier when context is deliberate, governance is centralized, and verification is mandatory.

Scaling Claude Code is not about giving everyone access and hoping for the best. It is about standardizing the inputs — CLAUDE.md, rules, skills, MCP, hooks — so that Claude Code delivers consistent, high-quality results across every engineer on the team.

---

## Rollout Timeline (Suggested)

| Week | Action                                                   |
| ---- | -------------------------------------------------------- |
| 1    | Create CLAUDE.md template, pilot with 2-3 engineers      |
| 2    | Add scoped rules and essential hooks                     |
| 3    | Configure MCP integrations, create custom agents         |
| 4    | Team-wide workshop, full rollout                         |
| 5+   | Iterate based on feedback, add skills as patterns emerge |
