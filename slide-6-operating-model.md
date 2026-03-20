# Chapter 6: Claude Code Operating Model

## Claude Code Is an Agent with Tools, Memory, Delegation, Agent Teams, and a Verification Loop

---

Claude Code is not a chatbot — it is an **agent** with four core capabilities.

---
### C — Context
---

Context is how Claude understands your project. It comes from:

- **CLAUDE.md** — your project's persistent memory. Architecture decisions, coding standards, build commands, team conventions. This loads at every session start.
- **Rules** — scoped guidance files that activate based on file paths. For example, a rule that says "all files in /api/ must have OpenAPI annotations."
- **@ file references** — explicit pointers to specific files. Instead of saying "fix the auth bug," say "fix the auth bug in @src/auth/handler.ts."
- **/context** — a diagnostic command that shows exactly what is loaded in the current session and how much context budget is being used.

---
### T — Tools
---

Claude Code has access to powerful tools:

- **Read, Edit, Write** — file manipulation
- **Bash** — execute shell commands, run tests, interact with your system
- **Git** — commit, branch, diff, log — full version control
- **Hooks** — deterministic shell and HTTP triggers at lifecycle events
- **MCP** — external tool integration via the Model Context Protocol

---
### M — Memory
---

Claude Code has an **auto memory** system. It writes timestamped notes between sessions — things like preferences, project decisions, and feedback. When a new session starts, relevant memories are loaded automatically.

Custom memory directories are also supported. Explicitly tell Claude to remember things: "Remember that our staging environment uses port 3001" — and it will recall that in future sessions.

---
### D — Delegation
---

This is the orchestration layer. Claude Code can spawn:

- **Built-in subagents** — Explore (fast search), Plan (research during planning), General-purpose (complex multi-step tasks)
- **Custom agents** — defined in `.claude/agents/*.md` with scoped tools, specific models, and custom hooks
- **Agent Teams** — multiple agents collaborating across git worktrees for cross-module work

---
### The Verification Loop
---

The most important mental model:

> **Main session holds decisions and final changes. Offload noisy work to isolated subagents.**

The main conversation is precious — it has the context of what is being achieved, the decisions made, and the final state of the code. Do not pollute it with log analysis, test output, or exploratory searches. Delegate those to subagents that return clean summaries.

### The Rule of Thumb

> **Don't ask Claude to be correct. Ask it to prove correctness with tests and evidence.**

This is the single most important principle for enterprise Claude Code usage. Never accept "I fixed the bug" at face value. Require "I fixed the bug, here is the test that proves it, and here is the output showing it passes."

---

## Example: Operating Model in Action

```
You: Fix the auth token expiry bug.
     Read @src/auth/handler.ts and @src/auth/token.ts.
     Run existing auth tests. Add a regression test.

Claude Code:
  1. [Context] Loads CLAUDE.md, reads the two referenced files
  2. [Tools] Identifies the bug — expiry comparison uses < instead of <=
  3. [Tools] Edits the file, writes a regression test
  4. [Delegation] Spawns subagent to run the full auth test suite
  5. [Verification] Reports: "12/12 tests pass, including new regression test"
  6. [Tools] Shows the diff for your review
```

This is the loop: Context -> Tools -> Delegation -> Verification. Every task follows this pattern.

---

## CLAUDE.md Best Practices

`CLAUDE.md` is the single most important file for Claude Code adoption. It is your project's **persistent onboarding document** — loaded at every session start, it tells Claude who it is working with, what the project looks like, and how to behave.

### Where CLAUDE.md Files Live

| Location                | Scope        | Use Case                                                                  |
| ----------------------- | ------------ | ------------------------------------------------------------------------- |
| `CLAUDE.md` (repo root) | Project      | Architecture, conventions, build commands — shared by the whole team      |
| `src/api/CLAUDE.md`     | Directory    | Module-specific context — loaded only when Claude works in that directory |
| `~/.claude/CLAUDE.md`   | User         | Personal preferences — applies across all projects for this user          |
| Org-managed CLAUDE.md   | Organization | Org-wide standards — pushed via MDM, applies to all users                 |

### Structure Guidelines

1. **Keep it lean** — CLAUDE.md loads at every session start. Every line costs context. Target **under 100 lines** for the root file.
2. **Lead with what Claude needs most** — build/test commands and architecture at the top, conventions below.
3. **Use "do not" directives** — explicitly tell Claude what to avoid scanning or modifying.
4. **Move bulky guidance to rules** — if a convention applies only to specific file types, use a scoped rule instead.
5. **Version control it** — commit CLAUDE.md to git so the whole team benefits and changes are reviewed.
6. **Update it as the project evolves** — treat it like living documentation, not a one-time setup.

### Recommended Structure

```markdown
# Project Name

## Tech Stack
[Languages, frameworks, databases, key libraries]

## Architecture
[Directory structure, layer responsibilities, data flow]

## Key Commands
[Build, test, lint, deploy — the commands Claude will run most]

## Code Style and Conventions
[Response shapes, naming patterns, validation approach]

## Navigation Rules
[Where to look for specific task types, what to never scan]

## Development Workflow
[Branching strategy, PR requirements, CI expectations]
```

### Example: Complex Enterprise E-Commerce Platform

```markdown
# ShopFlow — Enterprise E-Commerce Platform

## Tech Stack
- Backend: TypeScript, Node.js 20, Express, Zod validation
- Frontend: React 18, Next.js 14 (App Router), Tailwind CSS
- Database: PostgreSQL 16 (primary), Redis 7 (cache + sessions)
- Queue: BullMQ on Redis for async jobs (emails, inventory sync)
- Payments: Stripe SDK v14 — never use deprecated charge API
- Search: Elasticsearch 8 for product catalog
- Infrastructure: AWS (ECS Fargate, RDS, ElastiCache, S3)

## Architecture
- /src/api/          — REST endpoints (one file per resource, e.g., products.ts, orders.ts)
- /src/services/     — Business logic layer (NO direct DB imports — always go through repositories)
- /src/repositories/ — Database access via Knex query builder (never raw SQL strings)
- /src/jobs/         — BullMQ job processors (email, inventory, reporting)
- /src/middleware/    — Auth, rate limiting, error handling, request logging
- /src/events/       — Domain event publishers and subscribers
- /src/frontend/     — Next.js app (App Router, server components by default)
- /src/shared/       — Types, constants, and utilities shared between backend and frontend
- /migrations/       — Knex migrations (never modify existing migrations, always create new)
- /scripts/          — Deployment, seed data, and maintenance scripts

## Key Commands
- npm test                        — runs vitest (unit + integration)
- npm run test:e2e                — runs Playwright end-to-end tests
- npm run lint                    — eslint + prettier
- npm run typecheck               — tsc --noEmit
- npm run migrate                 — runs pending database migrations
- npm run migrate:rollback        — rolls back last migration batch
- npm run seed                    — seeds development database
- npm run build                   — builds both backend and frontend
- npm run dev                     — starts backend (port 3000) and frontend (port 3001)

## Code Style and Conventions
- All API responses use: { data, error, meta } shape (see @src/shared/types/api.ts)
- Use Zod schemas for ALL request validation — no manual parsing
- Repository pattern for all database access — services never import Knex directly
- Domain events for cross-module side effects (e.g., OrderPlaced → send email + update inventory)
- Feature flags via LaunchDarkly — check @src/shared/flags.ts for current flags
- Money values stored as integers (cents) — never use floats for currency
- All dates stored as UTC in PostgreSQL, converted to user timezone in the frontend only

## Navigation Rules
- Read @src/api/ and @src/services/ before any API task
- Read @src/repositories/ before any database task
- Read @src/middleware/auth.ts before any authentication task
- Read @migrations/ before creating a new migration
- Read @src/events/ before adding cross-module side effects
- Never scan node_modules/, dist/, .next/, coverage/, or .turbo/
- Never scan files larger than 500 lines without explicit instruction
- Never modify files in /scripts/deploy/ without human confirmation

## Development Workflow
- Branch from main, PR required, minimum 1 approval
- CI must pass: lint + typecheck + unit tests + e2e tests
- Migrations require DBA review if they touch tables > 1M rows
- Stripe integration changes require security team review
- All new API endpoints must have OpenAPI JSDoc annotations
- Feature branches: feature/<ticket-id>-<short-description>
- Commit messages: conventional commits (feat:, fix:, chore:, etc.)

## Known Gotchas
- Redis connection pool exhausts under load if batch jobs run during peak hours — use separate pool for jobs
- Elasticsearch reindex takes 45 min for full catalog — use incremental updates
- Stripe webhook handler assumes idempotency — always check event.id before processing
- Next.js App Router caching is aggressive — use revalidateTag() for dynamic product pages
```

### Directory-Level CLAUDE.md Example

For modules that need additional context, add a CLAUDE.md inside the directory:

```markdown
# /src/api/CLAUDE.md — API Endpoint Conventions

- Every endpoint file exports a single Express router
- Use @src/middleware/validate.ts for Zod request validation
- Use respondSuccess() and respondError() from @src/api/utils.ts
- Include OpenAPI JSDoc annotations on every route handler
- Rate limiting is applied globally in middleware — do not add per-endpoint limits
  unless explicitly required (see @src/middleware/rateLimit.ts for overrides)
- Pagination: use cursor-based pagination, not offset-based
  (see @src/api/products.ts for reference implementation)
```

### Anti-Patterns to Avoid

| Anti-Pattern                                 | Why It Hurts                                | Better Approach                                                  |
| -------------------------------------------- | ------------------------------------------- | ---------------------------------------------------------------- |
| Dumping entire API docs into CLAUDE.md       | Burns context on every session              | Move to a rule or skill that loads on demand                     |
| Listing every file in the project            | Claude can discover files itself            | List directory purposes, not individual files                    |
| Duplicating what is already in code comments | Wastes context on redundant info            | Reference the file instead: "see @src/shared/types.ts"           |
| No "do not scan" directives                  | Claude wastes tokens exploring node_modules | Always include explicit exclusions                               |
| Updating CLAUDE.md without PR review         | Bad instructions affect the whole team      | Treat CLAUDE.md changes like code changes                        |
| One massive CLAUDE.md (300+ lines)           | Dilutes signal, burns context budget        | Split into root CLAUDE.md + directory-level files + scoped rules |

---

## Useful Links

- [Claude Code — Official Documentation](https://code.claude.com/docs/en/overview)
- [Claude Code Sub-Agents](https://code.claude.com/docs/en/sub-agents)
- [Claude Code Memory (CLAUDE.md)](https://code.claude.com/docs/en/memory)
- [Building Agents with the Claude Agent SDK](https://www.anthropic.com/engineering/building-agents-with-the-claude-agent-sdk)
- [Agent SDK Overview](https://platform.claude.com/docs/en/agent-sdk/overview)
- [Claude Code Sandboxing](https://www.anthropic.com/engineering/claude-code-sandboxing)
