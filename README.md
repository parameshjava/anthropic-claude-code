# Claude Code for Enterprise Engineering

A comprehensive training guide for deploying Claude Code effectively across enterprise engineering teams. This guide covers everything from model selection to multi-agent orchestration, prompt engineering to CI/CD integration.

## Who This Is For

- Engineering teams adopting Claude Code for the first time
- Tech leads standardizing AI-assisted development workflows
- Engineering managers deploying Claude Code across multiple teams
- Individual developers looking to get the most out of Claude Code

## What You'll Learn

This guide is structured as 23 chapters, each covering a key aspect of using Claude Code in production engineering environments. Chapters 1-6 cover models and how Claude Code operates; Chapters 7-10 are the **Operating Discipline** module — how to stay in control of an agent that is allowed to act on its own; Chapters 11-23 cover the deeper technical workflows (context engineering, prompting, subagents, MCP, deployment, and CI/CD).

The finalized deck contains 52 slides. The [`slides/`](slides/) directory contains exactly
one presentation-ready Markdown companion for each slide, numbered `01` through `52`, so presenters can
jump from any slide to a concept-focused explanation, practical workflow, and current supporting sources.
The 23 top-level chapter files remain the deeper training guide; the slide companions are the
one-to-one reference layer rather than a second copy of every chapter.

## Deck Companion Map

The slide companions are organized as a delivery flow, not as a duplicate chapter index:

| Slides                                                                | Module               | What It Covers                                                                                                    |
| --------------------------------------------------------------------- | -------------------- | ----------------------------------------------------------------------------------------------------------------- |
| [1–6](slides/slide-01-claude-code-for.md)                             | Foundations          | Claude Code, model selection, epic decomposition, session pillars, and the operating model                        |
| [7–17](slides/slide-07-four-disciplines-one-idea.md)                  | Operating Discipline | Permissions, planning, auto mode, evidence, durable controls, budgets, operational commands, and official plugins |
| [18–19](slides/slide-18-context-engineering-what-should-load-when.md) | Context Engineering  | What should load, when it should load, and how to keep live sessions specific                                     |
| [20–41](slides/slide-20-claude-code-token-economics.md)               | Token Economics      | Tool-call loops, token categories, caching, model and effort choices, context cost, and managed observability     |
| [42–52](slides/slide-42-prompt-formula-for-claude-code.md)            | Apply and Scale      | Prompting, safe edits, feature selection, subagents, MCP, governance, hooks, CI, security, and adoption           |

Slides 15–17 are intentionally detailed operational references: cost and quality controls,
the commands engineers use during a session, and a curated official-plugin guide with installation
and invocation patterns.

## Table of Contents

| Chapter | Title                                                                       | What It Covers                                                                                                              |
| ------- | --------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| 1       | [Claude Code for Enterprise Engineering](slide-1-enterprise-intro.md)       | Introduction — what Claude Code is and why it matters for enterprise teams                                                  |
| 2       | [Claude Code Models by Plan](slide-2-models-by-plan.md)                     | Pro, Max, Team, and Enterprise plans, the four-model lineup with IDs, context, and pricing, and what it costs per developer |
| 3       | [Which Model for Which Task?](slide-3-model-selection.md)                   | Opus as the starting point, Fable for long-horizon work, Sonnet for cost, Haiku for speed                                   |
| 4       | [Epic Decomposition & Cheat Sheet](slide-4-epic-decomposition.md)           | Converting JIRA/Rally epics into Claude-ready stories with a task-to-model map                                              |
| 5       | [What This Session Covers](slide-5-session-overview.md)                     | Six pillars for deploying Claude Code effectively                                                                           |
| 6       | [Claude Code Operating Model](slide-6-operating-model.md)                   | How Claude Code works — tools, memory, delegation, agent teams, verification loop                                           |
| 7       | [Permission Modes](slide-7-permission-modes.md)                             | Who approves what — Manual, Accept Edits, Plan, Auto, Don't Ask, and Bypass, and the auto mode classifier                   |
| 8       | [Context Management](slide-8-context-management.md)                         | The context window as a budget — and why forgetting is a safety problem, not just a cost problem                            |
| 9       | [Managing Quality](slide-9-managing-quality.md)                             | A claim is not evidence — demand proof, run a verification loop, automate the checks that matter                            |
| 10      | [Human Accountability](slide-10-human-accountability.md)                    | The durability ladder for controls, and why a boundary is only as strong as where you wrote it down                         |
| 11      | [Context Engineering](slide-11-context-engineering.md)                      | Load only what the task needs — CLAUDE.md, rules, @ references, and context hierarchy                                       |
| 12      | [Manage Context — Avoid Limits](slide-12-manage-context.md)                 | Stop Claude from scanning everything — force specific file paths and manage token budgets                                   |
| 13      | [Prompt Formula for Claude Code](slide-13-prompt-formula.md)                | Six-part prompt formula (Goal, Files, Constraints, Scope, Validation, Output) with best practices checklist                 |
| 14      | [Reduce Hallucinations and Unsafe Edits](slide-14-reduce-hallucinations.md) | Six-step trust pipeline — Ground, Plan, Evidence, Validate, Review, Escalate                                                |
| 15      | [Which Feature Solves Which Problem?](slide-15-feature-decision-matrix.md)  | CLAUDE.md vs Rules vs Skills vs MCP vs Hooks — decision matrix with skills marketplace guide                                |
| 16      | [Subagents — Isolate, Specialize, Parallelize](slide-16-subagents.md)       | Built-in agents, custom agents, parallel execution patterns, and agent teams                                                |
| 17      | [MCP — Connect to Every Tool](slide-17-mcp.md)                              | Model Context Protocol — GitHub, Jira, Slack, databases, and reviewed community/vendor servers                              |
| 18      | [Enterprise Deployment and Governance](slide-18-enterprise-deployment.md)   | Anthropic Cloud, AWS Bedrock, Google Vertex AI, Microsoft Foundry deployment options                                        |
| 19      | [Scaling Claude Code Across Teams](slide-19-scaling-teams.md)               | Standardize inputs so Claude Code scales beyond one person                                                                  |
| 20      | [Hooks — Automate the Agent Lifecycle](slide-20-hooks.md)                   | Deterministic lifecycle triggers — PostToolUse, PreToolUse, Stop hooks                                                      |
| 21      | [GitHub Actions — Claude in CI/CD](slide-21-github-actions.md)              | Automated PR reviews, test generation, and issue-to-PR workflows                                                            |
| 22      | [Claude Code Security](slide-22-security.md)                                | Model-assisted vulnerability analysis that complements conventional security tooling                                        |
| 23      | [Reference & FAQ](slide-23-reference-faq.md)                                | Summary, resources, and frequently asked questions                                                                          |

## Key Concepts at a Glance

### The Prompt Formula (Chapter 13)

Every effective Claude Code prompt has six parts: **Goal** (what to achieve), **Files** (where to look), **Constraints** (rules to follow), **Scope** (what NOT to touch), **Validation** (how to prove it works), **Output** (what done looks like).

### The Trust Pipeline (Chapter 14)

Six steps to prevent hallucinations: **Ground** (read actual code, ask don't assume), **Plan** (preview approach), **Evidence** (require file paths and test output), **Validate** (automate via hooks), **Review** (summarize changes before committing), **Escalate** (flag uncertainty).

### Feature Decision Matrix (Chapter 15)

- **Every task needs it?** → CLAUDE.md
- **Only certain file types?** → Rules
- **Multi-step repeatable workflow?** → Skills
- **External system integration?** → MCP
- **Must happen, no exceptions?** → Hooks

### Skills, Agents, and MCP (Chapters 15-17)

- **Skills** provide the methodology (TDD, debugging, brainstorming) — run inside your session
- **Agents** provide isolation (PR review, test runs, investigations) — run independently
- **MCP** provides connections (JIRA, Slack, GitHub, databases) — external system access

### The Durability Ladder (Chapters 7-10)

Every control on Claude has a **durability**, unrelated to how firmly you phrased it: a spoken boundary can vanish on `/compact`, but `permissions.ask`, `permissions.deny`, and hooks are read from a file every time. On Pro, Max, and Team plans a new session starts in **auto mode** — a second model approves actions for you unless you tell it otherwise. **Context management is therefore a safety control**, a claim of "done" is not evidence until Claude proves it, and accountability is a matter of _where you wrote the rule down_, not how clearly you said it.

## Getting Started

1. **Start with Chapter 6** if you want to understand how Claude Code works under the hood
2. **Start with Chapter 7** if you already have Claude Code working and need to use it responsibly on a real codebase
3. **Start with Chapter 13** if you want to write better prompts immediately
4. **Start with Chapter 15** if you want to set up your project's CLAUDE.md, rules, and skills
5. **Start with Chapter 16** if you want to leverage subagents and parallel workflows

For the presentation path, begin with [Slides 1–6](slides/slide-01-claude-code-for.md), move through
[Slides 7–19](slides/slide-07-four-disciplines-one-idea.md) before enabling autonomous workflows,
use [Slides 20–41](slides/slide-20-claude-code-token-economics.md) to tune cost without removing
verification, and finish with [Slides 42–52](slides/slide-42-prompt-formula-for-claude-code.md) for
repeatable engineering and rollout patterns.

## Project Structure

```
anthropic-claude-code/
├── README.md                                   ← You are here
├── slide-1-enterprise-intro.md                 ← Chapter 1: Introduction
├── slide-2-models-by-plan.md                   ← Chapter 2: Models by Plan
├── slide-3-model-selection.md                  ← Chapter 3: Model Selection
├── slide-4-epic-decomposition.md               ← Chapter 4: Epic Decomposition
├── slide-5-session-overview.md                 ← Chapter 5: Session Overview
├── slide-6-operating-model.md                  ← Chapter 6: Operating Model
├── slide-7-permission-modes.md                 ← Chapter 7: Permission Modes
├── slide-8-context-management.md               ← Chapter 8: Context Management
├── slide-9-managing-quality.md                 ← Chapter 9: Managing Quality
├── slide-10-human-accountability.md            ← Chapter 10: Human Accountability
├── slide-11-context-engineering.md             ← Chapter 11: Context Engineering
├── slide-12-manage-context.md                  ← Chapter 12: Manage Context
├── slide-13-prompt-formula.md                  ← Chapter 13: Prompt Formula
├── slide-14-reduce-hallucinations.md           ← Chapter 14: Reduce Hallucinations
├── slide-15-feature-decision-matrix.md         ← Chapter 15: Feature Decision Matrix
├── slide-16-subagents.md                       ← Chapter 16: Subagents
├── slide-17-mcp.md                             ← Chapter 17: MCP
├── slide-18-enterprise-deployment.md           ← Chapter 18: Enterprise Deployment
├── slide-19-scaling-teams.md                   ← Chapter 19: Scaling Teams
├── slide-20-hooks.md                           ← Chapter 20: Hooks
├── slide-21-github-actions.md                  ← Chapter 21: GitHub Actions
├── slide-22-security.md                        ← Chapter 22: Security
├── slide-23-reference-faq.md                   ← Chapter 23: Reference & FAQ
├── slides/                             ← Exactly 52 one-to-one deck-slide companions
│   ├── slide-01-claude-code-for.md
│   ├── slide-07-four-disciplines-one-idea.md
│   ├── slide-15-a-budget-is-not-a-boundary.md
│   ├── slide-17-plugins-install-the-discipline-don-t-re-remember-it.md
│   ├── slide-20-claude-code-token-economics.md
│   ├── slide-42-prompt-formula-for-claude-code.md
│   └── slide-52-anthropic-claude-code.md
├── docs/token-economics.md                     ← Evidence-based companion to the deck's token section
├── agents/                                     ← Custom agent definitions
│   ├── gh-pr-reviewer.md                       ← PR review agent (Sonnet, posts to GitHub)
│   └── year-in-review.md                       ← Employee YIR generator (Sonnet, GitHub + JIRA)
├── skills/                                     ← Skill definitions (installable SKILL.md format)
│   ├── review/SKILL.md                         ← Code review checklist skill
│   ├── sec-review/SKILL.md                     ← Security review checklist skill
│   └── update-docs/SKILL.md                    ← Documentation update skill
├── diagram-assets/                             ← Source images for the deck's one diagram slide
│   ├── blueprint.png                           ← Mock blueprint (no licensing)
│   ├── hut.png                                 ← "Dream" reference image
│   └── villa.png                               ← "Built" reference image
├── samples/                                    ← Example outputs
│   └── sample-year-in-review.md                ← Sample YIR report (anonymized)
├── scripts/                                    ← Tooling
│   ├── check-docs.py                           ← Documentation consistency checks
│   ├── deck_kit.py                             ← Shared drawing toolkit for the deck
│   ├── renumber-slides.py                      ← Resync page numbers after reordering slides
│   ├── merge_operating_discipline_deck.py      ← One-off merge of the Operating Discipline slides
│   ├── build_token_economics_slides.py         ← Built the Token Economics section (native slides)
│   ├── consolidate_deck.py                     ← Removes redundant merged slides and resyncs footers
│   ├── generate_slide_companions.py             ← Generates the one-to-one Markdown slide references
│   ├── build-operating-discipline-slides.py    ← Built the commands and plugins slides
│   ├── add-plan-mode-analogy-slide.py          ← Built the plan-mode analogy slide
│   ├── rebuild-slide3-diagram.py               ← Draws the analogy slide's two flow diagrams
│   ├── make-mock-blueprint.py                  ← Generates the blueprint image (no licensing)
│   └── add-cost-control-slide.py               ← Built the spend-durability slide
│   ├── redesign_scaling_slide.py                ← Refines the scaling-teams slide
│   └── restore_plan_mode_analogy.py             ← Restores the plan-mode analogy slide
├── docs/superpowers/                           ← Design specs and implementation plans
├── Claude_Code_Agentic_Engineering_Session.pptx ← Slide deck (52 slides)
└── CLAUDE.md                                   ← Project-level Claude instructions
```

## Operating Discipline: Cheat Sheet & Further Training

Chapters 7-10 teach **discipline** — how to stay in control of an agent that,
on some paid plans and surfaces, may start with broader action permissions
without asking you first. Read them in order the first time: Chapter 7
establishes that a classifier may be approving actions for you, Chapter 8
shows how a boundary you set can silently disappear, Chapter 9 shows what the
classifier never checks, and Chapter 10 shows where to put a control so it
holds.

### Cheat Sheet

Pin this to your team channel.

**Which mode?**

| Situation                                     | Mode               | How                                 |
| --------------------------------------------- | ------------------ | ----------------------------------- |
| Unfamiliar codebase, or sensitive work        | Manual             | `Shift+Tab` to `⏸ manual mode on`   |
| You don't yet know what the change should be  | Plan               | `Shift+Tab`, or prefix with `/plan` |
| Well-scoped change, you're reading every diff | Accept edits       | `Shift+Tab`                         |
| Long task, you trust the direction            | Auto               | `Shift+Tab`                         |
| Auth, payments, migrations, infrastructure    | Manual             | `Shift+Tab`                         |
| CI with an exact allowlist                    | Don't ask          | `--permission-mode dontAsk`         |
| A container you will throw away               | Bypass permissions | `--dangerously-skip-permissions`    |

`⏸` means it holds back. `⏵⏵` means it runs. Check your status bar.

**Which control?**

| You want to…                                 | Use                    | Where                       |
| -------------------------------------------- | ---------------------- | --------------------------- |
| State a convention for this project          | `CLAUDE.md`            | Repo root, committed        |
| Be prompted before pushes and PRs            | `permissions.ask`      | `.claude/settings.json`     |
| Make something impossible                    | `permissions.deny`     | Managed settings            |
| Check the full command text, however written | `PreToolUse` hook      | `.claude/settings.json`     |
| Stop the classifier blocking internal work   | `autoMode.environment` | `~/.claude/settings.json`   |
| Cap what one CI run can spend                | `--max-budget-usd`     | on a `claude -p` invocation |

Anything said only in the conversation can be lost to `/compact`. A budget
nobody wrote into a file is in the same category — see [Chapter 10: Human
Accountability](slide-10-human-accountability.md#spend-has-a-ladder-too).

**Which command?**

| Need                                  | Command                   |
| ------------------------------------- | ------------------------- |
| See what is eating my context         | `/context`                |
| Trim history, keep what matters       | `/compact <what to keep>` |
| Start a completely different task     | `/clear`                  |
| Undo a wrong path                     | `/rewind`                 |
| Review everything before committing   | `/diff`                   |
| Check for vulnerabilities             | `/security-review`        |
| See what auto mode blocked, and retry | `/permissions`            |
| Plan one prompt, then resume          | `/plan <task>`            |
| Find where the spend is going         | `/usage`                  |

**The prompt that gets evidence**

```
<what is wrong, specifically>
Read @<file> and @<file>.

Identify the root cause and quote the lines responsible.
Add a test that fails before your fix and passes after.
Run it and paste the output, before and after.
Do not change <the thing that must not change>.

If you cannot reproduce it, stop and tell me — do not guess.
```

**Before you commit**

1. `/diff` — read the whole shape, not the edits as they went by
2. `/security-review` — if it touches auth, payments, or user data
3. Describe the change in your own words. If you can't, you haven't reviewed it.

### Official Anthropic Training That Pairs With Chapters 7-10

For engineers:

| Chapter                   | Official companion                                                                                                                                          | What it adds                                                                        |
| ------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| 7 · Permission Modes      | [Claude Code in Action](https://www.coursera.org/learn/claude-code-in-action) — _Module 2: Permission Modes_                                                | Matching autonomy to each of the six modes, hands-on                                |
| 8 · Context Management    | [Claude Code in Action](https://www.coursera.org/learn/claude-code-in-action) — _Module 1: Steer the Work_                                                  | Directed `/compact`, rewind checkpoints, long sessions                              |
| 9 · Managing Quality      | [Claude Code in Action](https://www.coursera.org/learn/claude-code-in-action) — _Verification Skills_, _Trust It: Verifying Unsupervised Runs_              | Building a check that "runs tests, reads diffs, and reports evidence automatically" |
| 10 · Human Accountability | [Steering Claude Code](https://claude.com/blog/steering-claude-code-skills-hooks-rules-subagents-and-more) · [Hooks](https://code.claude.com/docs/en/hooks) | Which mechanism enforces which kind of rule                                         |

Suggested order if someone is starting cold:
[Claude Code 101](https://academy.claude.com/) → Chapters 7-10 → **Claude Code
in Action** → [Agent Skills](https://academy.claude.com/) → Subagents.

For team leads and platform owners:

| Resource                                                                                                                                       | Use it for                                                                                                                       |
| ---------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| [Champion kit](https://code.claude.com/docs/en/champion-kit)                                                                                   | A **30-day adoption playbook** with weekly success signals, plus scripted answers to the seven objections you will actually hear |
| [Set up and govern Claude Code](https://academy.claude.com/products/code/setup)                                                                | A deployment wizard that generates a tailored rollout guide, plus Slack/Teams/email templates                                    |
| [The AI-Native SDLC Playbook](https://claude.com/blog/the-ai-native-sdlc-playbook)                                                             | Governance enforced as the agent acts — hooks as approval gates, PR as audit record                                              |
| [Claude Enterprise Administrator Guide](https://academy.claude.com/tutorials/claude-enterprise-administrator-guide)                            | The four rollout phases: technical setup, launch, enablement, scaling                                                            |
| [Scaling Agentic Coding Across Your Organization](https://resources.anthropic.com/scaling-agentic-coding)                                      | Moving past power users; measuring ROI beyond vanity metrics                                                                     |
| [How Claude Code works in large codebases](https://claude.com/blog/how-claude-code-works-in-large-codebases-best-practices-and-where-to-start) | Monorepo `CLAUDE.md` hierarchy, and the "agent manager" ownership role                                                           |
| [Rakuten case study](https://claude.com/customers/rakuten)                                                                                     | A named enterprise reference point — 12.5M-line codebase, seven-hour autonomous run                                              |

If you are the one running the rollout, the
[Enterprise Administrator Guide](https://academy.claude.com/tutorials/claude-enterprise-administrator-guide)
structures it in four phases — **technical setup → change management and launch
→ enablement and training → scaling adoption** — with concrete defaults worth
copying:

- **All-staff 101 workshops of 30-60 minutes**, covering navigation, prompting, and core features
- **2-3 champions per team or department**, equipped for peer support
- **Refresh training materials quarterly**, because the product moves

### The Evidence Base

Worth reading before you argue with a sceptical team — or before you promise
leadership a productivity number.

- **[DORA 2025: Balancing AI tensions](https://dora.dev/insights/balancing-ai-tensions/)** — ~5,000 professionals. 90% use AI; **30% report little to no trust in AI-generated code**; AI is associated with _increasing_ delivery instability. Names the **verification tax**: time saved writing is re-spent auditing.
- **[How we built Claude Code auto mode](https://www.anthropic.com/engineering/claude-code-auto-mode)** — users approved **93% of permission prompts anyway**, which is the approval-fatigue problem auto mode exists to solve. Also the classifier's measured false-negative rate.
- **[OWASP Top 10 for Agentic Applications](https://genai.owasp.org/2025/12/09/owasp-top-10-for-agentic-applications-the-benchmark-for-agentic-security-in-the-age-of-autonomous-ai/)** — **ASI09: Human-Agent Trust Exploitation.** Confident, polished explanations misled human operators into approving harmful actions. Human-in-the-loop is necessary but _not sufficient_.
- **[NIST AI RMF](https://www.nist.gov/itl/ai-risk-management-framework)** — the GOVERN function, and its known gap: it does not differentiate systems by degree of autonomy.

> **The one-line version for a slide:** DORA found that AI _amplifies_ what a
> team already has. Strong verification gets multiplied. So does the absence of
> it.

## Custom Agents Included

This project includes two production-ready custom agents as real-world examples:

- **gh-pr-reviewer** — Reviews GitHub PRs with inline comments, JIRA ticket alignment, codebase consistency checks, and suggestion blocks. Posts directly to GitHub.
- **year-in-review** — Generates employee performance reviews by aggregating data from GitHub, JIRA, and email. Produces balanced, manager-ready reports.

Both are covered in detail in [Chapter 16](slide-16-subagents.md). Agent definitions are in the [`agents/`](agents/) folder.

## Skills

Three working skills ship in this repo, in the installable `SKILL.md` format described in
[Chapter 15](slide-15-feature-decision-matrix.md):

| Skill                                                        | Purpose                                    |
| ------------------------------------------------------------ | ------------------------------------------ |
| [`skills/review/SKILL.md`](skills/review/SKILL.md)           | Code review checklist for React/Next.js    |
| [`skills/sec-review/SKILL.md`](skills/sec-review/SKILL.md)   | Security review checklist (OWASP)          |
| [`skills/update-docs/SKILL.md`](skills/update-docs/SKILL.md) | Bring docs back in sync after code changes |

### Skills Marketplace

Chapter 15 includes a comprehensive guide to the skills ecosystem:

- **Official:** https://github.com/anthropics/skills
- **Superpowers framework:** https://github.com/obra/superpowers
- **Community directory:** https://github.com/VoltAgent/awesome-agent-skills
- **Vendor-official skills** from Vercel, Cloudflare, Stripe, Netlify, HashiCorp, and more

## Contributing

To add or update a chapter:

1. Edit the corresponding `slide-N.md` file
2. Keep content practical — prefer examples over theory
3. Include real prompts, real commands, and real output
4. Update this README's table of contents if adding new chapters
5. Run `python3 scripts/check-docs.py` before committing — it verifies links resolve, skill and
   agent frontmatter parses, model versions stay in Chapter 2, this tree stays accurate, and
   every chapter carries its reading-time signpost and footer nav

### Chapter depth

Chapters currently range from ~2 to ~23 minutes of reading. Chapters 1, 2, 3, 5, and 17 are the
thinnest and are the best candidates for expansion; a 4–8 minute target is a reasonable goal for
each. Chapters 7, 11, 13, and 16 are the longest and may eventually warrant splitting — but the
23-chapter numbering is referenced by the slide deck, so any split needs a deliberate decision.

## Useful Links

- [How a Meta PM ships products without ever writing code | Zevi Arnovitz](https://youtu.be/1em64iUFt3U)
- [How to write a great agents.md](https://github.blog/ai-and-ml/github-copilot/how-to-write-a-great-agents-md-lessons-from-over-2500-repositories/)
- [Zevi's AI Development Workflow](https://shorthaired-billboard-f9a.notion.site/Zevi-s-AI-Development-Workflow-2c86baffbc90810fa63bd0ee8ecffce9#d2cdc296766c4ae5aabb0599e22850b8)
- [Agent Skills](https://agentskills.io/home)
