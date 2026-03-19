# Chapter 2: Claude Code Models by Plan

## Pro, Max, Team, and Enterprise Unlock Claude Code. Free Tier Provides Chat Only.

---

Before using Claude Code effectively, it is important to understand what is available at each subscription tier.

Claude Code is available across **four subscription plans**: Pro, Max, Team, and Enterprise. The free tier provides chat only — no agentic coding capabilities.

### Subscription Tiers

**Pro** is the entry point. It includes Sonnet as the default model, with Opus available for escalation. This tier is ideal for individual developers who want to start using Claude Code in their daily workflow.

**Max** raises the ceiling with higher usage limits, priority access to new features, and full Opus access. This tier is designed for power users who spend their entire day inside Claude Code.

**Team** introduces multi-user capabilities with admin controls. This is where organizations typically start — it includes shared settings, centralized billing, and Opus for the whole team.

**Enterprise** is the full governance suite — SSO, Role-Based Access Control, compliance features, and managed policies that IT can enforce centrally.

### The Three Models

The model lineup is critical to understand because choosing the right model for the right task is one of the biggest levers for productivity and cost efficiency.

**Haiku 4.5 — the Fast Specialist.** It is the fastest and most cost-efficient model. Use it for subagents that do parallel investigations, rapid prototyping, log summarization, and triage. Haiku functions as a lightweight worker that can run ten errands simultaneously.

For example, if scanning 50 log files for a pattern is required, dispatch Haiku subagents to do that in parallel rather than burning a main Opus session on it.

**Sonnet 4.6 — the Default Engineer.** This is the frontier coding model. It handles planning, knowledge work, code generation, debugging — essentially 80% of daily tasks. Sonnet is the default for Pro plans and the model to reach for first.

For example, "Fix the pagination bug in our search API" — Sonnet handles that perfectly. It reads the code, identifies the issue, writes the fix, and runs the tests.

**Opus 4.6 — the Principal Architect.** This is the strongest reasoning model. Use it when the problem is ambiguous, the architecture is complex, or the task requires long-horizon planning across multiple files and systems.

For example, "Redesign our authentication middleware to support multi-tenant SSO" — that is an Opus task. It needs to reason about trade-offs, read across many files, and make strategic decisions.

### The Operating Rule

The rule teams should internalize:

> **Use Sonnet 4.6 by default. Escalate to Opus 4.6 for ambiguity, architecture, or long-horizon work. Use Haiku 4.5 for speed, triage, and parallel subtasks.**

This is not about cost alone — it is about matching model strength to task complexity. Opus on a simple rename is overkill. Haiku on an architecture redesign will hallucinate.

---

## Examples for Client Teams

| Scenario                             | Model      | Why                        |
| ------------------------------------ | ---------- | -------------------------- |
| Fix a typo in error messages         | Haiku 4.5  | Fast, trivial change       |
| Implement a new REST endpoint        | Sonnet 4.6 | Standard development task  |
| Refactor monolith to microservices   | Opus 4.6   | Cross-cutting architecture |
| Summarize 200 test failures          | Haiku 4.5  | Speed + parallelism        |
| Debug a race condition in async code | Opus 4.6   | Deep reasoning required    |
