# Slide 43: Reduce hallucinations and unsafe edits

### Ground the change in evidence before accepting it

Hallucination and unsafe edits become less likely when the workflow makes unsupported assumptions visible and makes verification mandatory.

```text
Ground → Plan → Implement → Prove → Review → Escalate uncertainty
```

- **Ground:** name the symptom, relevant files, source of truth, and expected behavior. Ask Claude to quote the code or documentation that supports its conclusion.
- **Plan:** expose files, dependencies, risks, and out-of-scope work before editing cross-cutting changes.
- **Implement:** require the smallest change that addresses the established root cause.
- **Prove:** run the relevant test, build, contract check, or screenshot comparison; inspect the diff.
- **Review:** compare the result against the plan and acceptance criteria, preferably with a fresh reviewer for consequential work.
- **Escalate:** require Claude to state what it could not reproduce or verify. "I do not know" is safer than an invented fix.

### Example prompt

"Reproduce the login failure in @src/auth/. Quote the root-cause lines, propose the smallest fix, and do not edit until the plan is accepted. Add a failing regression test, run it before and after the change, show the diff, and list any assumption you could not verify."

Use permissions, hooks, and CI for actions that must be technically blocked. Prompting improves reasoning; it is not enforcement.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Skills](https://code.claude.com/docs/en/skills)

[← Previous slide](slide-42-prompt-formula-for-claude-code.md) · [Deck index](../README.md)