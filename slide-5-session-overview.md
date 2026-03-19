# Chapter 5: What This Session Covers

## Six Pillars for Deploying Claude Code Effectively Across Enterprise Engineering Teams

---

Everything is organized into **six pillars** — the building blocks for deploying Claude Code effectively at enterprise scale.

### Pillar 1: Context Engineering

This is the foundation. Context engineering means **loading only what Claude needs** — nothing more, nothing less.

A common mistake is letting Claude scan your entire project. That burns through your context window, slows responses, and introduces noise. Instead, use targeted file references with `@`, scoped rules that activate only in specific directories, and CLAUDE.md files that define your architecture and conventions upfront.

**Think of it this way:** you would not hand a new hire your entire codebase and say "read everything." You would give them the architecture doc, the relevant module, and the coding standards. The same principle applies to Claude Code.

### Pillar 2: Prompt Design

A prompt is not a wish — it is an engineering specification. The enforced formula is: **goal, scope, file paths, constraints, and validation** — every time.

Without this structure, Claude tends to wander. It might scan files you did not want it to touch, make changes beyond the requested scope, or skip validation. The prompt formula prevents all of that. A reusable skeleton that every engineer can adopt is covered in later chapters.

### Pillar 3: Delegation at Scale

Claude Code is not a single agent — it is an **orchestrator**. It can spawn subagents, each running in isolated contexts with their own tools and model selections. Agent Teams (currently in preview) enable multi-agent collaboration across git worktrees.

This means one agent can investigate logs, another can run tests, and a third can generate documentation — all in parallel, all isolated from the main session. This is how to scale from "one developer with Claude" to "Claude as a team member."

### Pillar 4: Connected Workflows (MCP)

The Model Context Protocol connects Claude Code to **everything** — Slack, JIRA, databases, Figma, GitHub, Sentry, and 3,000+ community servers. MCP servers are configured via `.mcp.json` files that you check into your repo, so the entire team gets the same integrations automatically.

Hooks add deterministic control: shell scripts and HTTP endpoints that fire at specific points in the agent lifecycle — before a tool runs, after an edit, when the session starts or stops. GitHub Actions extend this into your CI/CD pipeline.

### Pillar 5: Trust & Verification

This is non-negotiable for enterprise adoption. Claude Code must **prove** its work before you trust it.

That means requiring plans before implementation, demanding test output as evidence, running security scans on every change, and never committing without a human review step. Claude Code Security (launched February 2026) adds reasoning-based vulnerability scanning that catches issues traditional SAST tools miss.

### Pillar 6: Enterprise Governance

SSO, SCIM provisioning, managed permissions, spend controls, the Compliance API, and deployment options via AWS Bedrock, Google Vertex AI, or Microsoft Foundry.

This pillar ensures IT and security teams can deploy Claude Code with the same governance they apply to any other enterprise tool — centralized control, audit logs, data residency, and no training on your data.

---

## Key Takeaway

These six pillars are not optional — they are the minimum for responsible enterprise adoption. Skip any one of them and you get either a tool that underperforms (missing context/prompting) or a tool that cannot pass your security review (missing governance/verification).

---

## Useful Links

- [Claude Code — Official Documentation](https://code.claude.com/docs/en/overview)
- [Claude Code — Product Page](https://claude.com/product/claude-code)
- [Claude Code Best Practices](https://code.claude.com/docs/en/best-practices)
- [How Anthropic Teams Use Claude Code](https://www.anthropic.com/news/how-anthropic-teams-use-claude-code)
