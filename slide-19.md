# Chapter 19: Reference & FAQ

## Summary, Resources, and Frequently Asked Questions

---

### Guide Recap

This guide covered the following topics across the full Enterprise Engineering series:

1. **Models & Selection** — Haiku for speed, Sonnet by default, Opus for architecture. Match the model to the task, not the other way around.

2. **Context Engineering** — Load only what Claude needs. Use `@` file references, scoped rules, and CLAUDE.md. Keep always-on context lean.

3. **Prompt Formula** — Goal, files, constraints, scope, validation, output. Every time. No exceptions.

4. **Trust & Verification** — Ground, plan, evidence, validate, review, escalate. Never trust "looks right" — demand proof.

5. **Feature Selection** — CLAUDE.md for persistent context, rules for scoped guidance, skills for workflows, MCP for external tools, hooks for guarantees.

6. **Delegation** — Subagents isolate noisy work. Agent Teams parallelize across modules. Custom agents specialize for repeatable tasks.

7. **MCP Integration** — 3,000+ servers. Configure via `.mcp.json`. Check into repo for team-wide consistency.

8. **Enterprise Governance** — SSO, SCIM, spend controls, audit logs, managed policies. Deploy via Bedrock, Vertex, or Foundry for data residency.

9. **Scaling Playbook** — Six steps: lean CLAUDE.md, scoped rules, subagents, MCP config, hooks, and engineer onboarding.

10. **Hooks** — Six lifecycle events, four handler types, guaranteed execution. The backbone of enterprise safety.

11. **GitHub Actions** — Automated PR review, @claude mentions, test generation, issue-to-PR automation.

12. **Security** — Reasoning-based vulnerability scanning that finds what traditional SAST tools miss.

### Getting Started

For teams ready to adopt Claude Code, here is the recommended first week:

**Day 1-2:** Set up Claude Code access (Pro/Team/Enterprise plan). Create your first CLAUDE.md.

**Day 3-4:** Write 3-5 scoped rules for your most common file types. Configure 2-3 MCP integrations.

**Day 5:** Set up essential hooks (auto-lint, test runner, security gate). Run a team workshop on the prompt formula.

### Resources

- Claude Code documentation: https://code.claude.com/docs/en
- Best practices guide: https://code.claude.com/docs/en/best-practices
- MCP server directory: Browse the 3,000+ community servers
- Agent Skills: https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview
- Claude Code Security: https://code.claude.com/docs/en/security

---

## Frequently Asked Questions

**Q: What about data privacy?**
A: Anthropic does not train on your data. Enterprise plans offer VPC isolation via Bedrock/Vertex/Foundry. SOC 2 Type II certified. TLS 1.3 + AES-256 encryption.

**Q: How does pricing work?**
A: API-rate billing based on tokens consumed. Per-org and per-user spend caps available. Team and Enterprise plans include admin controls for budget management.

**Q: Can we use our own models alongside Claude?**
A: Claude Code is designed for Claude models. However, MCP integrations and hooks can connect to any external system, including other AI services for specific tasks.

**Q: How do we measure ROI?**
A: Track: time-to-merge for PRs, bug fix turnaround, test coverage improvements, and developer satisfaction scores. Most teams report 2-5x productivity gains in the first month.

**Q: What about offline/air-gapped environments?**
A: Bedrock and Vertex AI deployments can operate within your VPC. For fully air-gapped environments, contact Anthropic's enterprise team for options.

### Support Topics

For further assistance, the following areas are available for inquiry:
- **Adoption planning** — how to roll out Claude Code across an organization
- **Technical integration** — connecting to a specific tool stack
- **Security and compliance** — meeting governance requirements
- **Workflow optimization** — making Claude Code fit an engineering culture
