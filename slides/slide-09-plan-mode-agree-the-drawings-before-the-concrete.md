# Slide 9: Plan mode: agree the drawings before the concrete

### Plan mode is the blueprint review, not the code write

Imagine asking a builder to construct a house without agreeing the drawings. The builder may deliver a structurally sound house that still has the wrong layout, rooms, or foundation. Changing the drawing is inexpensive; changing the completed building means demolition, delay, and rebuilding.

Software has the same economics. A wrong interpretation is cheap to correct in a proposed plan, but expensive after it has become changed files, tests, migrations, documentation, reviews, and a deployment plan.

### The low-cost path: agree the drawing first

```text
Explore the current system
	↓
Review Claude's proposed files, approach, risks, and tests
	↓
Revise the plan until it matches the intended outcome
	↓
Approve the plan
	↓
Implement, verify, and review the change
```

- Plan mode exposes assumptions before the first source edit and maps affected layers, dependencies, risk, migration impact, rollback needs, and verification.
- When the design is wrong, say "No, keep planning" and correct the blueprint. This changes intent while the cost of change is still low.
- Approve only when the plan states what will change, what will not change, how success is verified, and where a human decision is still required.

### The expensive path: implement before agreement

- Claude begins coding from an incomplete request, makes a reasonable but wrong interpretation, and changes the controller, service, repository, tests, and possibly a migration.
- The team then discovers that the desired behavior, data contract, or rollout approach differs from the implementation.
- The result is rework: undoing code, repairing tests, revising migrations, repeating review, and rebuilding confidence in the release.

### Example: auth-flow change

Before implementation, ask: "In Plan mode, trace the current login and token-refresh flow. Propose the controller, service, repository, tests, migration impact, rollback plan, and acceptance criteria. Do not edit files until I approve the plan."

Review the proposal as a blueprint. If token expiry behavior, API compatibility, or rollback is wrong, revise the plan first; only then move to implementation.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Permission modes](https://code.claude.com/docs/en/permission-modes)
- [Permissions](https://code.claude.com/docs/en/permissions)
- [Memory](https://code.claude.com/docs/en/memory)

[← Previous slide](slide-08-the-six-permission-modes.md) · [Deck index](../README.md)