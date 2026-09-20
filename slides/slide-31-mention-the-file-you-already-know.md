# Slide 31: @-mention the file you already know

### If the file is already known, point to it directly

- `@` references give Claude a precise starting point and include the file rather than forcing a broad search to rediscover a path you already know.
- A file reference also brings the relevant directory guidance into scope, so the agent sees the local rules that govern the code it is about to change.

### Build context incrementally

1. Start with the implementation file, the matching test, and the observable symptom.
2. Add the interface, caller, schema, or configuration only when the evidence says the boundary matters.
3. Keep the target behavior explicit so Claude does not widen the task into an unnecessary refactor.

| Instead of | Prefer |
| --- | --- |
| "Find the login code and fix it" | "Read @src/auth/token.ts and @tests/auth/token.test.ts; reproduce expiry failure" |
| "Review the API" | "Review @src/controllers/OrdersController.cs for authorization and response-contract gaps" |
| "Find the migration" | "Inspect @src/migrations/20260920_add_status.sql and its repository caller" |

### Example

"Read @src/api/search.ts and preserve its response shape" is a low-cost, high-signal start. Add repository or serializer files only if the search behavior crosses those boundaries.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Context window](https://code.claude.com/docs/en/context-window)
- [Prompt caching](https://code.claude.com/docs/en/prompt-caching)
- [Claude Code costs](https://code.claude.com/docs/en/costs)

[← Previous slide](slide-30-a-larger-model-does-more-math-per-token.md) · [Deck index](../README.md)