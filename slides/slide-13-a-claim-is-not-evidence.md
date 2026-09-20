# Slide 13: A claim is not evidence

### Completion messages are not proof

- "Fixed it" and "tests pass" are claims. They may be true, but a reviewer cannot assess them without seeing what changed, which behavior was tested, and what actually ran.
- Evidence is reproducible and reviewable: the symptom, the relevant code or diff, the proof that failed before the change, the proof that passes after it, and the command output.
- Ask for evidence in the task itself. Without a runnable check, Claude stops when the work looks done and the engineer becomes the only verification loop.

### The evidence standard

| A completion claim | Reviewable evidence |
| --- | --- |
| "Fixed pagination." | Root cause in `pagination.ts`; regression test failed before and passed after; command and output attached |
| "The build is fixed." | Original error, root-cause change, `dotnet build` exit code, and no suppressed checks |
| "The API is correct." | Contract test, expected request/response cases, auth and error-path coverage, and test output |
| "The migration is safe." | Schema diff, migration and rollback notes, affected query checks, and staging or integration-test evidence |

### Build the verification loop into the task

```text
Symptom → identify and quote the root cause → write a failing regression test
	→ implement the smallest fix → run the test, build, and lint
	→ inspect the diff → report the evidence
```

- Require a test that **fails before** the fix and **passes after** it. A test that always passed does not prove the change fixed the reported behavior.
- Require the exact command and its result, not a paraphrase. Tests, builds, linters, type checks, screenshots, and behavior comparisons all count when they match the risk.
- Give Claude a safe exit: if it cannot reproduce the problem or establish proof, it must stop and report that uncertainty rather than guessing at a fix.

### Example: pagination defect

Instead of: "Fix pagination."

Ask: "Investigate duplicate rows on page two. Read @src/api/search.ts and @src/db/queries/pagination.ts. Quote the root-cause lines, add a regression test that fails before and passes after the fix, run the focused test and full relevant suite, and report the command output. Do not change the API response shape. If you cannot reproduce it, stop and say so."

### Evidence must match the change

- **Service logic:** focused unit tests, error and boundary cases, coverage where meaningful.
- **API or UI behavior:** contract or integration tests, screenshots, and expected request/response evidence.
- **Data or migrations:** schema and query impact, forward and rollback plan, staging or integration verification.
- **Security-sensitive changes:** adversarial review, authorization tests, dependency or secret checks, and explicit human approval.

### Keep the reviewer independent

- The agent that implemented the change should not be the only grader. Use a fresh subagent, CI gate, hook, or human reviewer to compare the diff against the plan and acceptance criteria.
- Reviewable evidence keeps engineering control with the team: it makes completion claims falsifiable before a pull request is accepted.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Permission modes](https://code.claude.com/docs/en/permission-modes)
- [Permissions](https://code.claude.com/docs/en/permissions)
- [Memory](https://code.claude.com/docs/en/memory)

[← Previous slide](slide-12-context-management-is-a-safety-control.md) · [Deck index](../README.md)