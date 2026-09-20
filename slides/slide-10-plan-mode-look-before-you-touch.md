# Slide 10: Plan mode — look before you touch

### Discovery and implementation are separate phases

Anthropic's recommended workflow is: **Explore → Plan → Approve → Implement → Verify and review**. Plan mode gives the engineer a deliberate checkpoint between understanding the system and changing it.

### Stage 1: Enter Plan mode and frame the task

- Start with `claude --permission-mode plan`, use the mode selector, or prefix one prompt with `/plan`.
- State the outcome, scope, constraints, and evidence required for success. Name important files with `@` rather than relying on broad exploration.
- In normal Plan-mode sessions, Claude can research but does not edit source files until the plan is approved.

```text
/plan Trace the current token-refresh flow in @src/auth/.
Propose the smallest fix. Preserve the public API, add a regression test,
and identify any migration or rollout risk. Do not edit files yet.
```

### Stage 2: Explore the existing system

- Claude reads the relevant code, tests, configuration, history, and existing patterns before proposing a solution.
- It should identify dependencies, affected layers, data contracts, security boundaries, migration impact, and existing verification commands.
- For a database change, this means examining the schema, current migration conventions, repositories, callers, test data, and rollback constraints.

### Stage 3: Produce a reviewable plan

- Require an ordered plan that names the files to change, the behavior to preserve, implementation steps, tests, risks, rollback approach, and work that is out of scope.
- A good plan explains why each layer changes: controller, service, repository, migration, configuration, and test suite.
- The plan is an artifact for engineering judgment, not a formality. If it cannot explain verification and rollback, it is not ready to implement.

### Stage 4: Review and revise before edits

- Read the proposed plan as you would a design review. Check that it matches the intended outcome, avoids unrelated changes, and covers security, data, performance, and operational risks.
- Use **"No, keep planning"** when the approach, scope, or assumptions are wrong. Edit the plan directly with `Ctrl+G` when useful, then ask Claude to refine it.
- Approve only when the blueprint is acceptable. Plan approval exits Plan mode and moves into the implementation mode you choose, such as manually approving edits or allowing a bounded automatic run.

### Stage 5: Implement, verify, and review

- Claude implements against the approved plan, writes or updates tests, runs the specified checks, and repairs failures.
- Require evidence: focused test output, build or lint result, coverage where applicable, the final diff, and any migration or rollout notes.
- The engineer reviews the pull request and accepts responsibility for the final decision; successful automation does not replace design, security, or release review.

### When Plan mode earns its overhead

Use it for uncertain designs, multi-file features, migrations, unfamiliar code, cross-layer refactors, or consequential changes. Skip it for a truly obvious, one-line change where the intended diff can be stated precisely.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Permission modes](https://code.claude.com/docs/en/permission-modes)
- [Permissions](https://code.claude.com/docs/en/permissions)
- [Memory](https://code.claude.com/docs/en/memory)

[← Previous slide](slide-09-plan-mode-agree-the-drawings-before-the-concrete.md) · [Deck index](../README.md)