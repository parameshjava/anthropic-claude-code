# Chapter 12: Subagents — Isolate, Specialize, and Parallelize Work

## Subagents Run in Isolated Contexts. Agent Teams Enable Multi-Agent Collaboration.

---

Subagents are one of Claude Code's most powerful capabilities, and also one of the most underused. The core concept: **your main conversation is your war room. Subagents are your field operatives.**

### The Orchestrator Model

The main conversation serves as the **orchestrator**. It holds:
- Your decisions and context about what you are trying to achieve
- The synthesis of information gathered by subagents
- The final code changes that will be committed

The main session should never be cluttered with verbose test output, log analysis, or exploratory searches. That is what subagents are for.

### Built-In Subagent Types

**Explore** — Fast, read-only search agent powered by Haiku. Use it to find files, search for patterns, or answer questions about the codebase. It keeps exploration noise out of your main context.

Example:
```
You: Use an Explore agent to find all files that import the
     deprecated SessionManager class.
Claude: [spawns Explore agent]
Result: Found 7 files importing SessionManager:
  - src/api/auth.ts:3
  - src/api/session.ts:1
  - src/middleware/auth.ts:5
  ... (clean summary returned to main session)
```

**Plan** — Research agent used during plan mode. It investigates the codebase to inform your plan without cluttering the planning conversation. Prevents infinite nesting of planning within planning.

**General-purpose** — The full-capability agent. It can explore, read, write, edit, and run commands. Use it for complex multi-step tasks that need tool access but should run independently.

Example:
```
You: Spawn a general-purpose agent to run the full test suite,
     analyze any failures, and return a summary of what broke and why.
Claude: [spawns agent, runs tests, analyzes output]
Result: 3 failures in auth module:
  - test_token_refresh: expected 200, got 401 (token not refreshed)
  - test_session_extend: timeout after 5s (async issue)
  - test_logout: assertion error on cookie clearing
  Root cause: all related to the session refactor in commit abc123
```

### Custom Agents

You can define custom agents in `.claude/agents/*.md`:

```markdown
# .claude/agents/test-runner.md
---
name: test-runner
model: haiku
tools: [Bash, Read, Grep]
---

You are a test execution specialist.
Run the specified test suite, capture all output,
and return a structured summary:
- Total tests: pass/fail/skip
- Failed tests: name, assertion, likely cause
- Performance: any tests over 5s
```

Custom agents have scoped tools (this one cannot edit files), a specific model (Haiku for speed), and custom instructions.

### When to Use Subagents

| Scenario | Why |
|----------|-----|
| Task produces verbose output | Keep noise out of main context |
| Work is self-contained | Agent can complete it without your main session's context |
| You need tool restrictions | Agent only gets the tools it needs |
| Investigations can run in parallel | Multiple agents search simultaneously |

### Agent Teams (Preview)

Agent Teams take this further — multiple agents collaborating across **git worktrees**. Each agent gets its own isolated copy of the repository and can make changes independently. The orchestrator merges results.

Use case: You need to update the auth module, the billing module, and the notification module. Each change is independent. Spawn three agents, each working in a separate worktree, each making their changes in parallel. The orchestrator reviews and merges all three.

---

## Example: Parallel Investigation

```
You: I'm seeing intermittent 500 errors in production.
     Investigate in parallel:
     1. Check the error logs for patterns
     2. Check the database slow query log
     3. Check the Redis connection pool metrics

Claude: [spawns 3 Explore subagents in parallel]

Agent 1 (logs): Errors cluster between 2-4am UTC, all from /api/reports
Agent 2 (DB): Slow query: report_generation taking 45s during batch jobs
Agent 3 (Redis): Connection pool exhaustion at 2am when batch starts

Synthesis: The batch job at 2am triggers expensive report queries that
exhaust the Redis connection pool and cause cascading 500 errors.
Recommendation: Move batch to a separate connection pool or queue.
```

Three investigations that would have taken 15 minutes sequentially — completed in parallel in under a minute, with a clean synthesis.
