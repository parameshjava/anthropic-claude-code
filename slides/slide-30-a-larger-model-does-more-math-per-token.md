# Slide 30: A larger model does more math per token

### Bigger models are more capable, but not always the best fit

- A stronger model can spend more reasoning on ambiguity, multi-step dependencies, competing designs, and synthesis across a large change surface.
- A smaller or faster model is often sufficient for bounded classification, formatting, boilerplate, log triage, or a clearly specified local edit.

| Task shape | Better starting point | Why |
| --- | --- | --- |
| Rename, format, narrow search, log classification | Haiku or low effort | The expected answer is constrained |
| Well-specified implementation or routine bug | Sonnet | Strong implementation with efficient cost |
| Ambiguous design, multi-file defect, cross-layer change | Opus | More reasoning for uncertainty and trade-offs |
| Long-horizon synthesis or unusually complex planning | Fable | Sustained reasoning across a large problem |

### Measure outcome, not model prestige

- Start from the deck's model-selection policy, then adjust model or effort when evidence shows the task needs more or less reasoning.
- Compare completion quality, rework, verification failures, and reviewer time. A lower per-token price is not a win if it creates repeated attempts.

### Example

Use a small model to classify failing tests, Sonnet to repair a contained service, and Opus to decide whether a distributed authentication failure is a local defect or an architecture problem.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Context window](https://code.claude.com/docs/en/context-window)
- [Prompt caching](https://code.claude.com/docs/en/prompt-caching)
- [Claude Code costs](https://code.claude.com/docs/en/costs)

[← Previous slide](slide-29-four-things-to-optimize.md) · [Deck index](../README.md)