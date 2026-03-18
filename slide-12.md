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

Claude Code ships with **six** built-in subagent types. Each is purpose-built for a specific class of work.

---

#### 1. Explore — Fast Codebase Search

**Purpose:** Read-only search and exploration at speed.
**Tools:** Glob, Grep, Read (read-only — cannot edit or execute)
**Best for:** Finding files, searching patterns, answering "where is X used?" questions.

The Explore agent supports three thoroughness levels: `quick` (basic search), `medium` (moderate exploration), `very thorough` (comprehensive analysis across multiple locations and naming conventions).

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

Example — thorough exploration:
```
You: Use an Explore agent (very thorough) to map out how our
     authentication flow works end-to-end — from login request
     to session creation to token refresh.
Claude: [spawns Explore agent with "very thorough" level]
Result: Authentication flow:
  1. POST /api/auth/login → src/api/auth.ts:45
  2. Validates credentials → src/services/authService.ts:23
  3. Creates session → src/services/sessionService.ts:67
  4. Stores in Redis → src/db/sessionStore.ts:12
  5. Issues JWT → src/utils/jwt.ts:34
  6. Refresh flow → src/middleware/tokenRefresh.ts:18
  All files listed with line numbers and key function signatures.
```

---

#### 2. Plan — Architecture and Strategy Research

**Purpose:** Investigate the codebase to inform implementation plans.
**Tools:** Glob, Grep, Read (read-only — research only, no edits)
**Best for:** Designing implementation strategies, identifying critical files, considering architectural trade-offs before writing code.

The Plan agent prevents infinite nesting of planning within planning. It researches and returns findings so your main `/plan` session stays focused on decisions, not exploration.

Example:
```
You: /plan Refactor the payment processing to support multiple providers
Claude: [spawns Plan agent to research]
Plan agent findings:
  - Current provider: Stripe only, hardcoded in src/services/payment.ts
  - 12 files directly reference Stripe SDK
  - Payment types defined in src/types/payment.ts (not provider-specific — good)
  - Webhook handler at src/api/webhooks/stripe.ts (provider-specific)
  - Suggested approach: Provider interface + adapter pattern
  - Risk: webhook handler needs per-provider routing
```

---

#### 3. General-Purpose — Full-Capability Independent Worker

**Purpose:** Complex, multi-step tasks that need full tool access but should run independently.
**Tools:** All tools (Read, Edit, Write, Bash, Glob, Grep, and more)
**Best for:** Running test suites, analyzing verbose output, performing multi-step tasks that would clutter the main session.

This is the most powerful subagent — it can do everything the main session can do, but in an isolated context.

Example — test analysis:
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

Example — migration safety check:
```
You: Spawn a general-purpose agent to:
     1. Run the pending migration against the test database
     2. Run the rollback
     3. Verify the schema is identical before and after
     4. Report any data loss risks
Claude: [spawns agent]
Result: Migration 20240315_add_soft_delete:
  - UP: adds deleted_at column to projects (nullable, no data loss)
  - DOWN: drops deleted_at column (⚠️ loses soft-delete data if any rows have values)
  - Schema diff after rollback: identical to pre-migration ✓
  - Risk: rollback after production use will lose deletion timestamps
```

---

#### 4. Code Reviewer — Implementation Validation

**Purpose:** Review completed code against plans and coding standards.
**Tools:** All tools (full access to read code, run checks, verify)
**Best for:** Validating that a major implementation step matches the original plan, checking for missed requirements, and catching quality issues.

Use this after completing a logical chunk of work — a feature, a refactor step, or any numbered step from your implementation plan.

Example:
```
You: I've finished implementing the user authentication system
     as outlined in step 3 of our plan. Review it.
Claude: [spawns code-reviewer agent]
Result:
  ✓ JWT token generation matches plan spec
  ✓ Refresh token rotation implemented
  ✗ Missing: rate limiting on /auth/login (plan step 3.4)
  ✗ Session invalidation on password change not implemented (plan step 3.6)
  ⚠ Token expiry set to 24h — plan specified 1h
  Recommendation: Address the 3 gaps before moving to step 4
```

---

#### 5. GitHub PR Reviewer — Pull Request Analysis

**Purpose:** Thoroughly review GitHub Pull Requests for quality, correctness, and impact.
**Tools:** Bash, Glob, Grep, Read, Edit, Write, WebFetch, WebSearch
**Best for:** Reviewing PRs before merge — checking code quality, identifying risks, assessing impact of proposed changes.

Example:
```
You: Review this PR: https://github.com/acme/api/pull/456
Claude: [spawns gh-pr-reviewer agent]
Result:
  PR #456: "Add rate limiting to upload endpoint"
  Files changed: 4 | +127 -12

  ✓ Rate limiter correctly uses sliding window algorithm
  ✓ Tests cover happy path, limit exceeded, and limit reset
  ✗ Missing: no test for concurrent requests hitting the limit simultaneously
  ⚠ Redis key expiry set to 60s but rate window is 120s — keys expire mid-window
  ⚠ No fallback if Redis is unavailable (endpoint will throw 500)
  Suggestion: Add a try/catch that allows requests through if Redis is down
              (fail-open for availability, log for monitoring)
```

---

#### 6. Claude Code Guide — Product Knowledge Expert

**Purpose:** Answer questions about Claude Code features, capabilities, and best practices.
**Tools:** Glob, Grep, Read, WebFetch, WebSearch
**Best for:** Questions about Claude Code itself — hooks, slash commands, MCP servers, settings, IDE integrations, keyboard shortcuts, the Agent SDK, and the Claude API.

Example:
```
You: How do I set up a PostToolUse hook that runs my formatter
     only on Python files?
Claude: [spawns claude-code-guide agent]
Result: Add this to .claude/settings.json:
  {
    "hooks": {
      "PostToolUse": [{
        "matcher": "Edit|Write",
        "command": "if [[ \"$FILEPATH\" == *.py ]]; then black \"$FILEPATH\"; fi"
      }]
    }
  }
  The matcher triggers on Edit or Write tool use.
  The command checks the file extension before running black.
```

### Custom Agents

Custom agents are where subagents become truly powerful for your team. You define them in `.claude/agents/*.md` with scoped tools, a specific model, and detailed instructions tailored to your workflow.

#### Anatomy of a Custom Agent

```markdown
# .claude/agents/my-agent.md
---
name: my-agent          # How you reference it
model: haiku            # haiku (fast/cheap), sonnet (balanced), opus (powerful)
tools: [Bash, Read]     # Only the tools this agent needs — less is safer
color: cyan             # Optional: terminal color for output
---

Your system prompt goes here. Tell the agent:
- What it is and what it does
- Step-by-step workflow
- Output format expectations
- Error handling rules
```

**Key design principles:**
- **Scope tools tightly** — an analyzer shouldn't have Edit access; a reviewer doesn't need Write
- **Choose the right model** — Haiku for fast/cheap tasks (search, test runs), Sonnet for complex reasoning (reviews, analysis), Opus for the most demanding work
- **Be specific in instructions** — the more detailed your system prompt, the more consistent the agent's output

#### Example 1: Test Runner (Simple — Haiku, Read-Only + Bash)

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

Fast, cheap, scoped — this agent cannot edit files, only run tests and report results.

#### Example 2: PR Reviewer (Complex — Sonnet, Multi-System Integration)

This is a real-world custom agent that reviews GitHub Pull Requests by combining code analysis with JIRA ticket alignment and codebase consistency checks.

```markdown
# .claude/agents/gh-pr-reviewer.md
---
name: gh-pr-reviewer
model: sonnet
tools: [Bash, Glob, Grep, Read, Edit, Write, WebFetch, WebSearch]
color: cyan
---

You are an expert GitHub Pull Request reviewer. When given a PR URL:

1. Fetch PR metadata, diff, and CI status using gh CLI
2. Fetch CLAUDE.md from the repo — all suggestions must align with project rules
3. Extract JIRA ticket ID from PR title/body/branch, fetch ticket details
4. Analyze story-to-code alignment (full match, partial, scope creep, misaligned)
5. Check codebase for duplicate logic the PR could reuse instead of reinventing
6. Post review to GitHub with inline comments and suggestion blocks

Prioritize findings:
- Critical: security vulnerabilities, data loss, breaking changes, logic errors
- Important: performance, architecture, missing error handling
- Nice to have: style, minor refactoring

Write like a senior engineer — direct, brief, no ceremony.
Post the review using gh api repos/.../pulls/.../reviews (NOT standalone comments).
```

**Why this is powerful:** A single `/pr-review https://github.com/org/repo/pull/123` triggers a multi-step workflow that:
- Reads the code diff via GitHub API
- Fetches the JIRA ticket for scope alignment
- Checks project rules from CLAUDE.md
- Searches the codebase for reusable code the PR missed
- Posts inline comments with suggestion blocks directly on GitHub

All of this runs in an isolated context — your main session stays clean.

#### Example 3: Year-in-Review Generator (Advanced — Sonnet, Multi-Source Data Aggregation)

This custom agent gathers contributions from GitHub, JIRA, and email to generate factual employee performance reviews.

```markdown
# .claude/agents/year-in-review.md
---
name: year-in-review
model: sonnet
tools: [Bash, Glob, Grep, Read, Write, WebFetch, WebSearch]
color: green
---

You are a pragmatic engineering manager writing Year-in-Review reports.
Given an employee name and year:

1. Resolve identity — map name to GitHub username and JIRA account
2. Gather GitHub data:
   - PRs authored (merged) — volume, repos, themes
   - PRs reviewed — count, quality of review comments
   - Commits and issues
3. Gather JIRA data:
   - Tickets completed by type (Story, Bug, Task)
   - Story points delivered
   - Epics/themes worked on
4. Gather email context (if Microsoft Graph is configured)
5. Analyze patterns — strengths, collaboration style, improvement areas
6. Write a balanced, factual report:
   - What they worked on (narrative)
   - Key contributions (with links)
   - How they work with the team
   - Areas to grow (backed by data, not speculation)
   - By the numbers (PRs, tickets, story points)
7. Save to ~/YIR/<name>-<year>-yir.md

Write like a human manager — no AI-speak, no inflation.
Frame improvement areas constructively with evidence.
```

**Why this is powerful:** A single prompt like `Generate a year-in-review for Alice for 2025` triggers an agent that:
- Queries the GitHub API for PR and review data across all repos
- Queries the JIRA API for completed tickets, story points, and comments
- Analyzes patterns across both systems (collaboration, consistency, growth areas)
- Produces a balanced, manager-ready report with real numbers and links
- Saves it to a file, ready for review

No copy-pasting between systems. No manual spreadsheet work. Data-driven, not vibes-driven.

#### Example 4: Security Auditor (Specialized — Sonnet, Restricted Tools)

```markdown
# .claude/agents/security-auditor.md
---
name: security-auditor
model: sonnet
tools: [Bash, Read, Grep, Glob]
---

You are a security auditor. Given a set of changed files:

1. Check for OWASP Top 10 vulnerabilities
2. Scan for hardcoded secrets, API keys, tokens
3. Verify input validation on all external boundaries
4. Check auth/authz patterns match project standards
5. Flag any new dependencies and check for known CVEs

Report format:
- CRITICAL: must fix before merge
- WARNING: should fix, acceptable risk if documented
- INFO: awareness only

Never edit files. Report findings only.
```

Note: no Edit or Write tools — this agent can only read and report, enforcing the principle that an auditor observes but does not modify.

#### The Custom Agent + MCP Connection

Custom agents often **use MCP connections** as part of their workflow. The relationship:

| Layer              | What It Provides                             | Example                                                                     |
| ------------------ | -------------------------------------------- | --------------------------------------------------------------------------- |
| **MCP** (slide 13) | Connections to external systems              | JIRA API, Slack API, GitHub API                                             |
| **Custom Agents**  | Workflows that orchestrate those connections | PR reviewer uses GitHub + JIRA MCP to fetch data, analyze, and post reviews |

MCP provides the **pipes**. Custom agents provide the **brains**. A PR reviewer agent without MCP connections cannot fetch JIRA tickets. MCP connections without a custom agent are just raw tools with no workflow.

#### Sharing Custom Agents with Your Team

Custom agents defined in `.claude/agents/` are committed to your repo — every developer who clones the repo gets the same agents. This means:

- Your PR review workflow is consistent across the team
- New team members get your specialized agents from day one
- Agent improvements are version-controlled and reviewable via PRs
- No per-developer setup needed beyond environment variables for MCP connections

### When to Use Subagents

| Scenario                           | Why                                                       |
| ---------------------------------- | --------------------------------------------------------- |
| Task produces verbose output       | Keep noise out of main context                            |
| Work is self-contained             | Agent can complete it without your main session's context |
| You need tool restrictions         | Agent only gets the tools it needs                        |
| Investigations can run in parallel | Multiple agents search simultaneously                     |

### Agent Teams (Preview)

Agent Teams take this further — multiple agents collaborating across **git worktrees**. Each agent gets its own isolated copy of the repository and can make changes independently. The orchestrator merges results.

Use case: You need to update the auth module, the billing module, and the notification module. Each change is independent. Spawn three agents, each working in a separate worktree, each making their changes in parallel. The orchestrator reviews and merges all three.

---

## Quick Reference: Which Agent for What?

| Agent             | Tools           | Read-Only? | Best For                                          |
| ----------------- | --------------- | ---------- | ------------------------------------------------- |
| Explore           | Glob,Grep,Read  | Yes        | Fast codebase search and pattern matching         |
| Plan              | Glob,Grep,Read  | Yes        | Research for implementation planning              |
| General-purpose   | All tools       | No         | Multi-step tasks with full tool access            |
| Code Reviewer     | All tools       | No         | Validating work against plans and standards       |
| GH PR Reviewer    | Most tools      | No         | Reviewing pull requests for quality and risks     |
| Claude Code Guide | Read,Web,Search | Yes        | Answering questions about Claude Code itself      |
| Custom (your own) | You choose      | You choose | Specialized, repeatable domain-specific workflows |

---

## Parallel Execution Patterns

The real power of subagents emerges when you run multiple agents simultaneously. Here are the key patterns.

### Pattern 1: Parallel Investigation (Multiple Explore Agents)

**When:** You need answers from different parts of the codebase simultaneously.

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

### Pattern 2: Parallel Implementation (Multiple General-Purpose Agents in Worktrees)

**When:** You have independent changes across different modules that don't conflict.

```
You: We need to add soft-delete support to three modules.
     Each is independent — parallelize with worktrees:
     1. Add soft-delete to the Projects module
     2. Add soft-delete to the Documents module
     3. Add soft-delete to the Comments module

     Each agent should:
     - Add deleted_at column migration
     - Update the repository to filter deleted rows by default
     - Add includeDeleted option for admin queries
     - Update existing tests and add soft-delete tests
     - Run that module's test suite

Claude: [spawns 3 general-purpose agents, each in isolated worktree]

Agent 1 (Projects): ✓ Migration, repository, tests — 14/14 passing
Agent 2 (Documents): ✓ Migration, repository, tests — 11/11 passing
Agent 3 (Comments): ✓ Migration, repository, tests — 9/9 passing

Claude (orchestrator): All three completed. Reviewing diffs for consistency...
  - All three use the same pattern ✓
  - Column type consistent (TIMESTAMP, nullable) ✓
  - Merging worktree branches into current branch...
```

### Pattern 3: Research Then Implement (Sequential Agent Coordination)

**When:** You need information from one agent before another agent can start working.

```
You: First, research how our notification system works.
     Then use those findings to implement email digest support.

Claude: [spawns Explore agent first]
Explore agent result:
  - Notifications created in src/services/notificationService.ts
  - Stored in notifications table with type, userId, payload
  - Real-time delivery via WebSocket in src/services/wsService.ts
  - No existing batch/digest mechanism
  - Email service exists at src/services/emailService.ts (used for password reset only)

Claude: [now spawns general-purpose agent with the research context]
You: Using the findings above, implement email digest support:
     - Aggregate unread notifications per user
     - Send digest email daily at 9am user-local-time
     - Use the existing emailService
     - Add a user preference to opt in/out

General-purpose agent: [implements with full context from research]
Result: 4 files created, 2 modified, migration added, 8 tests passing
```

### Pattern 4: Implement + Review (Agent Pipeline)

**When:** You want automated quality checks after implementation.

```
You: Implement the rate limiter for /api/upload (max 10/min per API key).
     After implementation, have a code reviewer validate the work
     against our plan.

Claude: [spawns general-purpose agent — implements the feature]
General-purpose agent result:
  - Created src/middleware/rateLimiter.ts (sliding window, Redis-backed)
  - Updated src/api/upload.ts to use middleware
  - Added 6 tests in src/api/__tests__/rateLimiter.test.ts
  - All tests passing

Claude: [spawns code-reviewer agent with the plan + diff]
Code reviewer result:
  ✓ Sliding window algorithm — correct
  ✓ Redis key structure — clean, uses apiKey:endpoint prefix
  ✓ Tests cover: under limit, at limit, over limit, window reset
  ✗ Missing: test for concurrent requests (plan item 4.3)
  ⚠ No fallback when Redis is unavailable — will 500
  Recommendation: Add try/catch with fail-open + alert

Claude: Addressing the two findings before committing...
```

### Pattern 5: Multi-Perspective Analysis (Different Agent Types in Parallel)

**When:** You want different types of analysis on the same change simultaneously.

```
You: I'm about to merge a large refactor of the payment module.
     Before I do, run these checks in parallel:
     1. Explore agent: Find all consumers of the old payment API
     2. General-purpose agent: Run the full test suite
     3. General-purpose agent: Run the security scan on changed files
     4. Code reviewer: Check the refactor against our architecture rules

Claude: [spawns 4 agents in parallel]

Agent 1 (Explore): 23 files consume the payment API. 21 already updated.
  ⚠ 2 files still reference old interface:
    - src/services/subscriptionService.ts:45
    - src/jobs/invoiceGenerator.ts:12

Agent 2 (Tests): 247/249 passing.
  ✗ test_subscription_renewal — TypeError: payment.createCharge is not a function
  ✗ test_invoice_generation — Cannot read property 'amount' of undefined

Agent 3 (Security): No new vulnerabilities. PCI-relevant changes reviewed.
  ✓ No credit card data logged
  ✓ Stripe keys still loaded from env only

Agent 4 (Code Review):
  ✓ Provider interface pattern matches architecture doc
  ✗ subscriptionService and invoiceGenerator not updated (confirms Agent 1)
  ⚠ Missing: adapter for PayPal (plan included it)

Synthesis: Two files missed in the refactor — causing the two test failures.
Fix those before merging. PayPal adapter still TODO.
```

### Pattern 6: Background Agent + Foreground Work

**When:** You want to keep working while a long-running task completes.

```
You: Run the full integration test suite in the background
     (it takes a few minutes). While that runs, let's work on
     the API documentation for the new endpoints.

Claude: [spawns general-purpose agent in background — runs tests]
Claude: Sure, let's work on the docs while tests run.
        Which endpoints should we document first?

... (you continue working on docs) ...

[Background agent completes]
Claude: Integration tests finished — 142/142 passing ✓
        No issues. We can continue with the docs.
```

---

## Anti-Patterns: Common Subagent Mistakes

| Mistake                                      | Better Approach                                                   |
| -------------------------------------------- | ----------------------------------------------------------------- |
| Using main session for large search tasks    | Spawn Explore agents — keep main context clean                    |
| Running agents sequentially when independent | Spawn in parallel — agents run simultaneously                     |
| Giving agents more tools than needed         | Scope tools in custom agents — an analyzer shouldn't edit         |
| Not providing enough context to the agent    | Include relevant file paths, constraints, and expected output     |
| Using a single agent for unrelated tasks     | One agent per concern — easier to parallelize and debug           |
| Forgetting to synthesize results             | The orchestrator must combine findings into actionable next steps |
