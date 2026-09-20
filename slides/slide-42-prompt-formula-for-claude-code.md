# Slide 42: Prompt formula for Claude Code

### A strong prompt is goal, scope, constraints, and proof

| Prompt element | What to provide | Why it matters |
| --- | --- | --- |
| **Goal** | Observable outcome | Prevents solving a different problem |
| **Scope** | Relevant files, symbols, and what is out of scope | Prevents broad scanning and unrelated edits |
| **Constraints** | Compatibility, architecture, security, performance, and approval boundaries | Makes non-negotiables explicit |
| **Proof** | Test, build, screenshot, or acceptance check | Defines done as evidence, not a claim |
| **Output** | Plan, diff summary, findings, or PR-ready result | Makes the response reviewable |

### Reusable formula

```text
Goal: [outcome]
Scope: read [@files]; do not modify [boundaries]
Constraints: preserve [contract]; follow [existing pattern]
Proof: add/run [test or command] and report the result
Output: summarize changed files, evidence, and remaining uncertainty
```

### Example

"Align JWT claims with RFC 7519 in @src/auth/handler.ts. Preserve the session API and do not modify token storage. Add a regression test for the invalid-claim path, run the focused auth suite, and return the changed files, command output, and any assumption you could not verify."

For uncertain or cross-cutting work, ask for a Plan-mode proposal first. A precise prompt is an engineering specification, not a wish.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Skills](https://code.claude.com/docs/en/skills)

[← Previous slide](slide-41-efficient-doesn-t-mean-fewer-tokens.md) · [Deck index](../README.md)