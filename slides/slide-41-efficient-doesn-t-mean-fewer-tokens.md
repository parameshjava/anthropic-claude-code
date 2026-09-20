# Slide 41: Efficient doesn't mean fewer tokens

### Efficiency is about outcomes, not raw token count

- Fewer tokens is not automatically better. A short session that guesses, skips validation, or creates a second repair task is wasteful even if the initial bill is small.
- More tokens can be the efficient choice when they buy the evidence that prevents rollback, review churn, outages, or repeated human investigation.

| Wasteful token use | Valuable token use |
| --- | --- |
| Broad scanning without a hypothesis | Reading the implementation and test that establish root cause |
| Replaying massive logs | Filtering to the error that drives the next decision |
| Repeatedly guessing at a fix | Planning, testing, and revising against evidence |
| Long stale sessions | Focused context for the current task |

### The efficiency test

The best workflow produces a correct, reviewable change at an appropriate safety bar with minimal **waste**: irrelevant context, repeated rework, unbounded output, and avoidable human correction.

### Example

A slightly longer session that identifies the real defect, adds a regression test, and shows passing evidence is more efficient than a short, plausible patch that returns as a production incident or review rejection.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Context window](https://code.claude.com/docs/en/context-window)
- [Prompt caching](https://code.claude.com/docs/en/prompt-caching)
- [Claude Code costs](https://code.claude.com/docs/en/costs)

[← Previous slide](slide-40-three-things-to-take-home.md) · [Deck index](../README.md)