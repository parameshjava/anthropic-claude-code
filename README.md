# Claude Code for Enterprise Engineering

A comprehensive training guide for deploying Claude Code effectively across enterprise engineering teams. This guide covers everything from model selection to multi-agent orchestration, prompt engineering to CI/CD integration.

## Who This Is For

- Engineering teams adopting Claude Code for the first time
- Tech leads standardizing AI-assisted development workflows
- Engineering managers deploying Claude Code across multiple teams
- Individual developers looking to get the most out of Claude Code

## What You'll Learn

This guide is structured as 19 chapters, each covering a key aspect of using Claude Code in production engineering environments. The chapters progress from foundational concepts (models, context) through advanced workflows (subagents, CI/CD, security scanning).

## Table of Contents

| Chapter | Title                                                       | What It Covers                                                                                              |
| ------- | ----------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| 1       | [Claude Code for Enterprise Engineering](slide-1.md)        | Introduction — what Claude Code is and why it matters for enterprise teams                                  |
| 2       | [Claude Code Models by Plan](slide-2.md)                    | Pro, Max, Team, and Enterprise plans — what each unlocks                                                    |
| 3       | [Which Model for Which Task?](slide-3.md)                   | Sonnet for daily work, Opus for complexity, Haiku for speed — model selection guide                         |
| 4       | [Epic Decomposition & Cheat Sheet](slide-4.md)              | Converting JIRA/Rally epics into Claude-ready stories with a task-to-model map                              |
| 5       | [What This Session Covers](slide-5.md)                      | Six pillars for deploying Claude Code effectively                                                           |
| 6       | [Claude Code Operating Model](slide-6.md)                   | How Claude Code works — tools, memory, delegation, agent teams, verification loop                           |
| 7       | [Context Engineering](slide-7.md)                           | Load only what the task needs — CLAUDE.md, rules, @ references, and context hierarchy                       |
| 8       | [Manage Context — Avoid Limits](slide-8.md)                 | Stop Claude from scanning everything — force specific file paths and manage token budgets                   |
| 9       | [Prompt Formula for Claude Code](slide-9.md)                | Six-part prompt formula (Goal, Files, Constraints, Scope, Validation, Output) with best practices checklist |
| 10      | [Reduce Hallucinations and Unsafe Edits](slide-10.md)       | Six-step trust pipeline — Ground, Plan, Evidence, Validate, Review, Escalate                                |
| 11      | [Which Feature Solves Which Problem?](slide-11.md)          | CLAUDE.md vs Rules vs Skills vs MCP vs Hooks — decision matrix with skills marketplace guide                |
| 12      | [Subagents — Isolate, Specialize, Parallelize](slide-12.md) | Built-in agents, custom agents, parallel execution patterns, and agent teams                                |
| 13      | [MCP — Connect to Every Tool](slide-13.md)                  | Model Context Protocol — GitHub, JIRA, Slack, databases, and 3,000+ community servers                       |
| 14      | [Enterprise Deployment and Governance](slide-14.md)         | Anthropic Cloud, AWS Bedrock, Google Vertex AI, Microsoft Foundry deployment options                        |
| 15      | [Scaling Claude Code Across Teams](slide-15.md)             | Standardize inputs so Claude Code scales beyond one person                                                  |
| 16      | [Hooks — Automate the Agent Lifecycle](slide-16.md)         | Deterministic lifecycle triggers — PostToolUse, PreToolUse, Stop hooks                                      |
| 17      | [GitHub Actions — Claude in CI/CD](slide-17.md)             | Automated PR reviews, test generation, and issue-to-PR workflows                                            |
| 18      | [Claude Code Security](slide-18.md)                         | Reasoning-based vulnerability scanning that finds zero-days traditional tools miss                          |
| 19      | [Reference & FAQ](slide-19.md)                              | Summary, resources, and frequently asked questions                                                          |

## Key Concepts at a Glance

### The Prompt Formula (Chapter 9)
Every effective Claude Code prompt has six parts: **Goal** (what to achieve), **Files** (where to look), **Constraints** (rules to follow), **Scope** (what NOT to touch), **Validation** (how to prove it works), **Output** (what done looks like).

### The Trust Pipeline (Chapter 10)
Six steps to prevent hallucinations: **Ground** (read actual code, ask don't assume), **Plan** (preview approach), **Evidence** (require file paths and test output), **Validate** (automate via hooks), **Review** (summarize changes before committing), **Escalate** (flag uncertainty).

### Feature Decision Matrix (Chapter 11)
- **Every task needs it?** → CLAUDE.md
- **Only certain file types?** → Rules
- **Multi-step repeatable workflow?** → Skills
- **External system integration?** → MCP
- **Must happen, no exceptions?** → Hooks

### Skills, Agents, and MCP (Chapters 11-13)
- **Skills** provide the methodology (TDD, debugging, brainstorming) — run inside your session
- **Agents** provide isolation (PR review, test runs, investigations) — run independently
- **MCP** provides connections (JIRA, Slack, GitHub, databases) — external system access

## Getting Started

1. **Start with Chapter 6** if you want to understand how Claude Code works under the hood
2. **Start with Chapter 9** if you want to write better prompts immediately
3. **Start with Chapter 11** if you want to set up your project's CLAUDE.md, rules, and skills
4. **Start with Chapter 12** if you want to leverage subagents and parallel workflows

## Project Structure

```
anthropic-claude-code/
├── README.md              ← You are here
├── slide-1.md             ← Chapter 1: Introduction
├── slide-2.md             ← Chapter 2: Models by Plan
├── ...
├── slide-19.md            ← Chapter 19: Reference & FAQ
├── .claude/
│   ├── agents/            ← Custom agent definitions
│   │   ├── gh-pr-reviewer.md
│   │   └── year-in-review.md
│   └── settings.json      ← Claude Code configuration
└── CLAUDE.md              ← Project-level Claude instructions
```

## Custom Agents Included

This project includes two production-ready custom agents as real-world examples:

- **gh-pr-reviewer** — Reviews GitHub PRs with inline comments, JIRA ticket alignment, codebase consistency checks, and suggestion blocks. Posts directly to GitHub.
- **year-in-review** — Generates employee performance reviews by aggregating data from GitHub, JIRA, and email. Produces balanced, manager-ready reports.

Both are covered in detail in [Chapter 12](slide-12.md).

## Skills Marketplace

Chapter 11 includes a comprehensive guide to the skills ecosystem:
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
