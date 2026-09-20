# Slide 15: A budget is not a boundary

### Cost control and safety control are different tools

- A spending or token budget answers: **"How much may this usage cost?"** It does not answer: **"May this command run?"**
- A safety boundary is enforced by permissions, sandboxing, hooks, CI, branch protection, and review. These controls decide whether an action or delivery may proceed.
- Use both controls together: cost controls limit consumption; the control plane limits capability and release risk.

| Concern                      | Correct control                     | Example                                        |
| ---------------------------- | ----------------------------------- | ---------------------------------------------- |
| Avoid unexpected token spend | Plan, workspace, or provider budget | Cap usage credits or API workspace spend       |
| Stop a force push            | Permission rule                     | `deny: ["Bash(git push --force *)"]`           |
| Require migration approval   | Human checkpoint                    | `ask: ["Bash(dotnet ef database update *)"]`   |
| Stop an untested merge       | Delivery gate                       | Required CI status check and code-owner review |

### Add local quality gates before and after commits

- Use a **pre-commit hook** for fast, deterministic checks: formatting, linting, type checks, secret detection, or focused tests. It catches obvious failures before a commit reaches review or CI.
- Use a **post-commit hook** for non-blocking follow-up work: show the commit summary, open the relevant test command, update local metadata, or remind the engineer to create a pull request.
- Keep hooks fast and deterministic. The CI pipeline and branch protection remain the final, shared merge boundary because local hooks can be skipped or misconfigured.

```text
pre-commit: format → lint → type-check → secret scan
post-commit: summarize commit → run optional local follow-up → prepare PR reminder
CI: full tests → security checks → required review → merge
```

### Preserve the work across long sessions

For multi-stage work, keep the plan, current status, and test results in explicit artifacts rather than relying only on conversation history. This gives each compaction a durable source of truth.

```text
Plan
	↓
Write plan.md
	↓
/compact
	↓
Implement
	↓
Update status.md
	↓
/compact
	↓
Test
	↓
Update results.md
	↓
/compact
```

- `plan.md`: intended scope, affected files, constraints, risks, and acceptance criteria.
- `status.md`: completed work, current changes, open questions, and the next implementation step.
- `results.md`: commands run, test/build output, failures fixed, remaining risks, and release or rollback notes.
- Commit durable project artifacts when they are useful to the team. Keep temporary task notes in a gitignored scratch location so they do not become accidental repository documentation.

### Model and effort handoffs: preserve context, avoid repeated cache rebuilds

- **Correction:** switching models with `/model` does not erase the conversation. Claude Code sends the same conversation history to the newly selected model, so it can continue the task.
- **Cost trade-off:** each model has its own prompt cache. A model or effort switch usually makes the next request reprocess the full conversation without cache hits, so repeatedly switching models in a long session can be slower and more expensive.
- Choose the model and effort at a natural phase boundary, then keep them stable while that phase runs. Record the handoff in `plan.md`, `status.md`, or `results.md` so the next phase starts with a concise, durable brief.

| Phase | Cost-effective default | Use a stronger setting when |
| --- | --- | --- |
| Frame the request | Haiku or low effort, in a separate short session if helpful | The requirements themselves contain difficult trade-offs |
| Analyze and plan | Opus at its default effort; Plan mode | The design is ambiguous, cross-layer, security-sensitive, or the model has not reasoned deeply enough |
| Implement a reviewed plan | Sonnet at its default effort | The implementation uncovers a hard dependency, unexpected risk, or a failed plan assumption |
| Verify and review | Focused tests and a fresh reviewer | The change is consequential or needs independent challenge |

### A validated long-task pattern

```text
Optional: use Haiku to turn rough notes into a clear requirement brief
	↓
Save the brief in plan.md
	↓
Use Opus in Plan mode to analyze risks and approve the implementation plan
	↓
Implement on Sonnet when the plan is well specified
	↓
Run tests, update results.md, and review the final diff
```

- Claude Code's official `opusplan` setting automates the central handoff: **Opus during Plan mode** and **Sonnet during execution**. The phase transition starts a fresh model cache, but it preserves the conversation and intentionally spends stronger reasoning where ambiguity is highest.
- Do not downgrade merely because a task is long. Keep Opus or escalate when the problem remains ambiguous or the smaller model has the right context but is still wrong.
- Do not raise effort by default. Anthropic recommends the model's default effort for most tasks; increase effort when the model skipped files, tests, or verification, and reduce it for a truly clear, bounded edit.

### Minimize cost by removing waste, not proof

| Cost driver                            | Better model behavior                                                   | Practical team control                                                       |
| -------------------------------------- | ----------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| Broad exploration                      | Start from known files and retrieve more only when evidence requires it | Use `@` references, scoped rules, and targeted prompts                       |
| Oversized always-on context            | Load only stable, high-signal guidance at startup                       | Keep root `CLAUDE.md` lean; move procedures to skills or path rules          |
| Overpowered reasoning for routine work | Match model and effort to uncertainty                                   | Use lower effort for bounded edits; reserve stronger reasoning for ambiguity |
| Verbose logs and tool output           | Read failures and summaries, not every line                             | Use focused tests, filters, files, or subagents for large diagnostics        |
| Stale sessions                         | Retain only the current task's decisions and evidence                   | `/compact` the same task with guidance; `/clear` unrelated work              |
| Unused external tools                  | Load the smallest tool surface needed for the workflow                  | Prefer CLI or narrow API access when sufficient; disable unused MCP servers  |
| Parallel work with no boundary         | Split only independent, high-value investigation                        | Keep subagents small, scoped, and shut them down after the result returns    |

- Plan uncertain work before implementation. A short planning pass can avoid expensive edit, test, and rework loops.
- Use objective verification as the stopping condition. Anthropic's engineering guidance favors mechanical tests, builds, diffs, and adversarial review over repeated speculative fixes.
- Inspect `/usage`, `/context`, and provider reporting before changing defaults; tune observed waste rather than guessing.

### Example: two separate failures

- A low-cost agent can still edit the wrong production configuration. The budget succeeded; governance failed.
- A well-governed agent can use more tokens while it investigates, adds regression tests, and verifies a difficult fix. The safety process succeeded; the team then manages the cost separately.

### Use budgets to improve the workflow

- Monitor `/usage` and provider or plan reporting for long contexts, cache misses, expensive model defaults, and unused MCP integrations.
- Reduce spend with targeted context, task-appropriate model and effort choices, and clean session boundaries.
- Never respond to cost pressure by removing tests, review, permission prompts, or release gates.

### Example

A monthly spend limit may stop overspending, but only permissions and review gates stop force-pushes or production edits.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Permission modes](https://code.claude.com/docs/en/permission-modes)
- [Permissions](https://code.claude.com/docs/en/permissions)
- [Memory](https://code.claude.com/docs/en/memory)
- [Cost management](https://code.claude.com/docs/en/costs)
- [Context window](https://code.claude.com/docs/en/context-window)
- [Model configuration](https://code.claude.com/docs/en/model-config)
- [Prompt caching](https://code.claude.com/docs/en/prompt-caching)
- [Anthropic: model and effort selection](https://claude.com/blog/claude-model-and-effort-level-in-claude-code)
- [Anthropic engineering: effective context engineering](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- [Anthropic engineering: AI code migrations](https://claude.com/blog/ai-code-migration)

[← Previous slide](slide-14-the-durability-ladder.md) · [Deck index](../README.md)
