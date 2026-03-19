# Chapter 4: Epic Decomposition & Cheat Sheet

## Converting JIRA/Rally Epics into Stories, Plus a Quick-Reference Task-to-Model Map

---

One of the highest-value use cases for Opus 4.6 in enterprise environments is **epic decomposition**.

Every engineering org faces this problem: a product owner writes an epic in JIRA or Rally — a few paragraphs of high-level requirements — and an engineering lead spends half a day converting that into well-structured user stories with acceptance criteria. Opus can do this in minutes.

### Why Opus for Epic Decomposition

This task requires capabilities that specifically match Opus's strengths:

- **Reading and interpreting product requirements** — Opus has a 200K context window and excels at extracting actionable work items from high-level product language
- **Decomposing ambiguous requirements into stories** — this requires judgment about what constitutes a single deliverable unit of work
- **Writing acceptance criteria per story** — each story needs testable, specific criteria that a developer can implement against
- **Identifying technical risks and stack limitations** — Opus understands architecture well enough to flag "this story depends on a database migration that will lock the table for 10 minutes"
- **Surfacing alternatives and trade-offs** — "we could use WebSockets here, but polling is simpler and our load balancer doesn't support sticky sessions"
- **Connecting product intent to technical implementation** — bridging the gap between what the PO described and what engineers need to build

### Example: Real-World Epic Decomposition

Suppose a product owner submits this epic in Rally:

> **Epic: Customer Notification Preferences**
>
> Customers should be able to choose which notifications they receive and how they receive them (email, SMS, in-app push). We need a preferences page in the settings area. Customers who opt out of marketing emails must still receive transactional emails (order confirmations, password resets). We also need to respect regional opt-in regulations (GDPR, CAN-SPAM). The system should support adding new notification channels in the future without a full rewrite.

The following prompt demonstrates how to invoke Opus for this task:

```
Read the following product epic from our PO.
Decompose into user stories with acceptance criteria.
Add dependencies, risks, limitations, alternatives,
and trade-offs for each story. Reference @src/notifications/
and @src/db/migrations/ for existing patterns.
Output as markdown with one H2 per story.

[paste epic description / epic link]
[Provide existing architecture and techstack information]
```

Attach the epic text directly or via `@` references to a requirements doc. Opus reads the product language, identifies the logical story boundaries, and produces a structured breakdown.

**Pro tip:** For long requirement documents, use Haiku as a pre-processor. Have Haiku subagents extract and structure each section (goals, constraints, compliance requirements) before sending to Opus. This reduces the token load on Opus and improves output quality.

### Task-to-Model Cheat Sheet

Pin this to your team's Slack channel:

| Model          | Role                | Use For                                                                               |
| -------------- | ------------------- | ------------------------------------------------------------------------------------- |
| **Haiku 4.5**  | Fast Specialist     | Log scanning, triage, scaffolding, parallel subtasks, summarization                   |
| **Sonnet 4.6** | Default Engineer    | Feature development, debugging, code review, test writing, refactoring                |
| **Opus 4.6**   | Principal Architect | Epic decomposition, architecture decisions, ambiguous problems, cross-cutting changes |

### Sample Opus Output

Opus produces the following from the notification preferences epic:

**Story 1:** Create notification_preferences table and migration
- *Acceptance criteria:* Migration adds user_id, channel (email/sms/push), category (marketing/transactional), enabled boolean. Default: all transactional enabled, marketing follows existing opt-in state.
- *Risk:* Backfill for existing users — bulk INSERT for 2M+ users may need batching to avoid table locks.

**Story 2:** Build preferences API endpoints (GET/PUT /users/:id/notification-preferences)
- *Depends on:* Story 1
- *Acceptance criteria:* Users can read and update their preferences per channel and category. Transactional emails cannot be disabled (enforced server-side, UI shows them as locked). Returns 403 if updating another user's preferences.

**Story 3:** Preferences UI in settings page
- *Depends on:* Story 2
- *Acceptance criteria:* Toggle grid (channels x categories). Transactional row is visible but non-toggleable with tooltip explaining why. Saves optimistically with rollback on failure.
- *Alternative:* Simple list of checkboxes vs. channel x category matrix — matrix is more scalable for future channels.

**Story 4:** Enforce GDPR/CAN-SPAM compliance rules
- *Depends on:* Story 1
- *Acceptance criteria:* EU users default to opt-out for marketing (GDPR). Unsubscribe link in every marketing email. Audit log for preference changes with timestamp and source.
- *Risk:* Region detection relies on user profile address — need fallback for users without address on file.

**Story 5:** Integrate preferences check into notification dispatch service
- *Depends on:* Stories 1, 2
- *Acceptance criteria:* Dispatch service queries preferences before sending. Transactional bypasses preference check. New channels can be added via config without code changes to the dispatch layer.
- *Trade-off:* Per-send DB lookup vs. cached preferences — cache adds complexity but avoids latency at scale.

This level of structured decomposition — with dependencies, risks, compliance flags, and trade-offs — would take a senior engineer hours. Opus produces it in minutes, and the engineer reviews and refines rather than creating from scratch.

---

## Key Takeaway

Epic decomposition is where Opus pays for itself. The cost of one Opus session is a fraction of the engineering hours saved on story writing, risk identification, and dependency mapping.

---

## Useful Links

- [Claude Code Best Practices](https://code.claude.com/docs/en/best-practices)
- [Choosing a Model](https://platform.claude.com/docs/en/about-claude/models/choosing-a-model)
- [Claude Prompt Engineering Overview](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/overview)
- [Enabling Claude Code to Work More Autonomously](https://www.anthropic.com/news/enabling-claude-code-to-work-more-autonomously)
