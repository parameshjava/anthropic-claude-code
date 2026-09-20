# Slide 50: GitHub Actions: Claude in your CI/CD pipeline

### CI turns Claude from a local assistant into a workflow participant

- A GitHub Actions workflow can use Claude for pull-request review, issue triage, mention-driven assistance, tests, or bounded implementation work.
- The workflow executes with repository permissions, tokens, secrets, and event data. Treat it as privileged automation, not as a chat feature.

### Safe CI design

| Control                                      | Why it matters                                                      |
| -------------------------------------------- | ------------------------------------------------------------------- |
| Least-privilege workflow permissions         | Limits what a compromised or mistaken workflow can change           |
| Trusted event handling                       | Avoids granting write capability to untrusted fork or comment input |
| Scoped secrets and environments              | Prevents an assistant workflow from inheriting deployment authority |
| Required status checks and branch protection | Keeps CI evidence and human approval before merge                   |
| Explicit output contract                     | Makes review comments or generated patches inspectable              |
| Pinned action revisions                      | Reduces action supply-chain drift                                    |
| Turn and timeout limits                      | Bounds runtime and spend for each workflow run                       |

### Useful patterns

- Read-only PR review that reports correctness or security gaps with file references.
- `@claude` mention that produces a bounded plan or issue analysis.
- Issue-to-PR automation only in a protected workflow with scoped permissions and mandatory review.

Do not pass an untrusted issue, PR, or comment directly into a write-capable agent workflow. Treat the content as untrusted input; constrain its authority, validate proposed changes, and keep secrets unavailable to fork-originated or otherwise untrusted runs.

### Example

Run a read-only PR review on every pull request. It reports findings and suggested tests, while existing test gates, code-owner approval, and branch protection remain the authority that determines whether the change merges.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [GitHub Actions](https://code.claude.com/docs/en/github-actions)
- [Code review](https://code.claude.com/docs/en/code-review)

[← Previous slide](slide-49-hooks-automate-every-step-of-the-agent-lifecycle.md) · [Deck index](../README.md)
