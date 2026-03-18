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

## Architecture (quick reference)
- /src/api/ — REST endpoints (one file per resource)
- /src/services/ — business logic (no direct DB imports)
- /src/db/ — repositories and migrations
- See docs/ARCHITECTURE.md for data flow and component diagrams

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

**When to create a separate architecture file:**

The brief folder overview in CLAUDE.md is enough for small, single-service projects. But as your project grows, create a dedicated `docs/ARCHITECTURE.md` (or `ARCHITECTURE.md` at the repo root) and reference it from CLAUDE.md. Use a separate architecture file when:

- **Multiple services or packages** — microservices, monorepos, or multi-package workspaces need a map of how services communicate
- **Data flow matters** — if understanding the request lifecycle (API → service → repository → database → cache) is critical for correct changes
- **Dependency boundaries exist** — "services never import from API layer," "shared types live in /src/types only" — these rules need diagrams or detailed explanation
- **Component relationships are non-obvious** — event-driven systems, message queues, pub/sub patterns, or background job pipelines

**Example `docs/ARCHITECTURE.md`:**
```markdown
# Architecture

## System Overview
Express REST API → Service Layer → PostgreSQL + Redis Cache

## Request Lifecycle
1. Request hits /src/api/{resource}.ts
2. Zod validates input
3. Service in /src/services/ applies business logic
4. Repository in /src/db/ executes query
5. Response formatted via respondSuccess/respondError helpers

## Layer Rules
- API layer: HTTP concerns only (validation, status codes, headers)
- Service layer: Business logic only (no req/res objects, no direct DB)
- DB layer: Data access only (no business decisions)
- Never skip a layer (API must not call DB directly)

## Key Data Flows
- Auth: JWT middleware → session service → Redis session store
- Search: API → search service → Elasticsearch → response cache (Redis, 5min TTL)
- File upload: API → S3 presigned URL → async processing via Bull queue
```

Then CLAUDE.md simply points to it — keeping CLAUDE.md concise while giving Claude access to the full picture when needed.

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

### 3. Skills & Plugins — Composable Expertise Packages

**Loads:** Via `/slash-command` or automatically when relevant

**Best for:** Repeatable workflows that involve multiple steps — TDD, code review, design, debugging, document generation.

Skills are the **Agent Skills** open standard created by Anthropic. A skill is a folder containing a `SKILL.md` file with YAML frontmatter and instructions. Claude loads them dynamically and auto-activates them based on conversation context. Skills run **inside your main session** — they augment your conversation with specialized expertise rather than running in isolation like subagents.

#### Skill File Format

A minimal skill:
```markdown
---
name: my-skill
description: What it does and when to use it
---
# Instructions for Claude

Step-by-step workflow here.
```

Skills live in:
- **Personal:** `~/.claude/skills/` (your skills, available everywhere)
- **Project:** `.claude/skills/` (team skills, committed to git)

#### Installing Skills from Marketplaces

The official skills repository is at **https://github.com/anthropics/skills** — Anthropic's curated collection with 96k+ stars.

**Install via the plugin system:**
```bash
# Add the official Anthropic marketplace
/plugin marketplace add anthropics/skills

# Install a specific plugin from the marketplace
/plugin install document-skills@anthropic-agent-skills

# Update all marketplaces
/plugin marketplace update
```

**Share marketplaces with your team** via `.claude/settings.json`:
```json
{
  "extraKnownMarketplaces": {
    "company-tools": {
      "source": { "source": "github", "repo": "your-org/claude-plugins" }
    }
  }
}
```

Every developer who clones the repo gets access to the same skill marketplaces automatically.

#### Official Anthropic Skills

From https://github.com/anthropics/skills:

| Category          | Skills                                                                       |
| ----------------- | ---------------------------------------------------------------------------- |
| **Document**      | `docx` (Word), `pdf`, `pptx` (PowerPoint), `xlsx` (Excel), `doc-coauthoring` |
| **Design**        | `algorithmic-art`, `canvas-design`, `slack-gif-creator`, `theme-factory`     |
| **Development**   | `frontend-design`, `web-artifacts-builder`, `mcp-builder`, `webapp-testing`  |
| **Communication** | `brand-guidelines`, `internal-comms`                                         |
| **Meta**          | `skill-creator` — use this to create your own custom skills                  |

#### Superpowers Framework (Highly Recommended)

**https://github.com/obra/superpowers** — a complete software development methodology with 15+ composable skills.

```bash
/plugin marketplace add obra/superpowers-marketplace
/plugin install superpowers@superpowers-marketplace
```

| Skill                            | Purpose                                                     |
| -------------------------------- | ----------------------------------------------------------- |
| `test-driven-development`        | Enforces RED-GREEN-REFACTOR TDD cycles                      |
| `systematic-debugging`           | Four-phase root cause analysis before proposing fixes       |
| `brainstorming`                  | Socratic design refinement before coding                    |
| `writing-plans`                  | Creates bite-sized 2-5 minute implementation tasks          |
| `executing-plans`                | Batch task execution with review checkpoints                |
| `subagent-driven-development`    | Two-stage code review (spec compliance + quality)           |
| `dispatching-parallel-agents`    | Concurrent multi-agent workflows                            |
| `requesting-code-review`         | Pre-review checklists before submitting                     |
| `receiving-code-review`          | Systematic feedback integration without blind agreement     |
| `using-git-worktrees`            | Isolated parallel development branches                      |
| `verification-before-completion` | Confirms fixes actually resolve issues before claiming done |

#### Vendor-Official Skills

Major platforms publish official skills maintained by their engineering teams:

| Vendor               | Skills                                                             | Source                  |
| -------------------- | ------------------------------------------------------------------ | ----------------------- |
| **Vercel**           | Next.js best practices, React patterns, web design, upgrade guides | `vercel-labs/*`         |
| **Cloudflare**       | Workers, Durable Objects, Wrangler, AI agents, MCP servers         | `cloudflare/*`          |
| **Netlify**          | Functions, edge functions, DB, image CDN, deploy, caching          | `netlify/*`             |
| **Stripe**           | Payment best practices, SDK upgrades                               | `stripe/*`              |
| **Supabase**         | Postgres best practices                                            | `supabase/*`            |
| **HashiCorp**        | Terraform code gen, module gen, provider development               | `hashicorp/*`           |
| **Expo**             | App design, deployment, upgrading                                  | `expo/*`                |
| **Google Labs**      | React components, shadcn-ui, design systems                        | `google-labs-code/*`    |
| **Google Workspace** | Drive, Sheets, Gmail, Calendar, Docs, Slides (24+ skills)          | `googleworkspace/cli`   |
| **Hugging Face**     | Datasets, model training, evaluation, tool building                | `huggingface/*`         |
| **Trail of Bits**    | Security auditing, secure smart contracts                          | `trailofbits/*`         |
| **Remotion**         | Programmatic video creation                                        | `remotion-dev/remotion` |
| **Sanity**           | Content modeling, SEO, content experimentation                     | `sanity-io/*`           |

#### Community Skill Collections

| Collection                                          | What It Offers                                              | Link                                                               |
| --------------------------------------------------- | ----------------------------------------------------------- | ------------------------------------------------------------------ |
| **VoltAgent/awesome-agent-skills**                  | 549+ curated skills from official vendors and community     | https://github.com/VoltAgent/awesome-agent-skills                  |
| **travisvn/awesome-claude-skills**                  | Curated directory of skills, resources, and tools           | https://github.com/travisvn/awesome-claude-skills                  |
| **levnikolaevich/claude-code-skills**               | 6 plugins, 125+ skills covering the full delivery lifecycle | https://github.com/levnikolaevich/claude-code-skills               |
| **jeremylongshore/claude-code-plugins-plus-skills** | 346 plugins, 1,900+ skills across 22 categories             | https://github.com/jeremylongshore/claude-code-plugins-plus-skills |
| **SkillsMP**                                        | Largest directory — 500k+ skills aggregated from GitHub     | https://skillsmp.com                                               |

#### Example: Using Skills in Practice

```
You: /test-driven-development
     Add a rate limiter to the /api/upload endpoint.
     Max 10 requests per minute per API key.

Claude: [skill activates TDD workflow in your main session]
  Step 1: Writing failing test...
    test_rate_limiter_allows_under_limit — RED ✗
    test_rate_limiter_blocks_over_limit — RED ✗
  Step 2: Implementing rate limiter...
    Created src/middleware/rateLimiter.ts
  Step 3: Running tests...
    test_rate_limiter_allows_under_limit — GREEN ✓
    test_rate_limiter_blocks_over_limit — GREEN ✓
  Step 4: Refactoring...
    Extracted config to constants, added JSDoc
  All tests still GREEN ✓
```

```
You: /systematic-debugging
     Users report that search returns duplicate results
     on page 2 of paginated queries.

Claude: [skill activates 4-phase debugging workflow]
  Phase 1 — Reproduce: wrote test confirming duplicates on page 2
  Phase 2 — Isolate: narrowed to OFFSET calculation in pagination.ts
  Phase 3 — Root cause: off-by-one when page=2 sets OFFSET=limit (not limit+1)
  Phase 4 — Fix: corrected offset formula, test now passes
  Verification: ran full search test suite — 23/23 passing
```

**Rule of thumb:** If it is a multi-step workflow you repeat across projects, make it a skill.

---

### Skills vs Agents vs MCP — When to Use Which

These three features serve different purposes but work together:

| Feature           | What It Is                                 | Runs Where                                                         | Use When                                                            |
| ----------------- | ------------------------------------------ | ------------------------------------------------------------------ | ------------------------------------------------------------------- |
| **Skills**        | Prompt templates with workflows            | **Inside** your main session                                       | You need a repeatable methodology (TDD, debugging, brainstorming)   |
| **Custom Agents** | Independent subprocesses with scoped tools | **Isolated** context, returns results                              | You need independent work that shouldn't clutter your session       |
| **MCP**           | Connections to external systems            | **Background** services, tools available to both skills and agents | You need to reach JIRA, Slack, databases, or other external systems |

**How they compose:**
- A **skill** (like `/test-driven-development`) orchestrates your main session
- That skill might **spawn agents** (like a test-runner subagent) for isolated work
- Those agents might **use MCP connections** (like a database server) to verify results

Skills provide the **methodology**. Agents provide the **isolation**. MCP provides the **connections**.

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

| Question                        | Answer -> Feature |
| ------------------------------- | ----------------- |
| Does every task need this?      | CLAUDE.md         |
| Only for certain file types?    | Rules             |
| Multi-step repeatable workflow? | Skills            |
| External system integration?    | MCP               |
| Must happen, no exceptions?     | Hooks             |
