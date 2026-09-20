# Slide 4: Epic decomposition & cheat sheet

### Turn a big request into reviewable stories

- Start from the epic, then break it into small, testable work items
- Each story should include outcome, scope, dependencies, risks, and verification
- This creates smaller delivery units and keeps the review loop tight
- Use model choice at the story level, not for the whole epic

### Best fit by model

- Opus: default for decomposition, ambiguity, trade-offs, cross-cutting planning
- Fable: multi-epic synthesis and long-horizon delivery planning
- Sonnet: well-specified implementation stories and routine follow-ups
- Haiku: triage, summaries, boilerplate, parallel subtasks

### Example

Turn “migrate billing to the new provider” into stories for API contract changes, data migration, retry behavior, observability, and rollback, each with acceptance criteria and a verification step.

### Cheat sheet

- Haiku — triage, summarization, scaffolding
- Sonnet — routine implementation and testing
- Opus — ambiguous engineering and decomposition
- Fable — architecture synthesis and large delivery plans

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Choosing a model](https://platform.claude.com/docs/en/about-claude/models/choosing-a-model)
- [Claude prompt engineering overview](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/overview)

[← Previous slide](slide-03-which-model-for-which-task.md) · [Deck index](../README.md)