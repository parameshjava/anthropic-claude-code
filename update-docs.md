---
description: Update documentation based on recent code changes
---

# Update Documentation Task

You are updating documentation after code changes. Follow these steps:

## 1. Identify Changes
- Check `git diff HEAD --name-only` and `git status --short` for modified files
- Identify which features/modules were changed
- Note any new files, deleted files, or renamed files

## 2. Verify Current Implementation
**CRITICAL**: DO NOT trust existing documentation. Read the actual code.

For each changed file:
- Read the current implementation
- Understand actual behavior (not documented behavior)
- Note any discrepancies with existing docs

## 3. Update Relevant Documentation

### Determine which doc categories are affected:

| Change type                        | Target file                            |
| ---------------------------------- | -------------------------------------- |
| Architecture, tech stack, patterns | `docs/architecture/system-overview.md` |
| Data model / schema / types        | `docs/architecture/data-model.md`      |
| API contracts (`route.ts` files)   | `docs/architecture/api-contracts.md`   |
| User-facing features               | `docs/features/<feature-name>.md`      |
| Dev setup, tooling, scripts        | `docs/development/setup.md`            |
| Env vars, deployment, hosting      | `docs/operations/deployment.md`        |

### For each affected category:
1. Check if the target file exists.
2. **If it exists** — update only the stale sections. Do not rewrite sections that are still accurate.
3. **If it does not exist** — create it from scratch based on the actual code. Do not skip it.
4. Always verify content against the real code before writing. Never assume.

### Skip a category only if:
- The change type genuinely does not apply (e.g., no `route.ts` files exist → skip `api-contracts.md`)
- State the reason explicitly in your response when skipping

### Required regardless of changes:
- **CHANGELOG.md** — add entry under "Unreleased" using categories: Added, Changed, Fixed, Security, Removed
  - Concise, user-facing language
  - Use The Good Docs CHANGELOG template

## 4. Documentation Style Rules

✅ **Concise** - Sacrifice grammar for brevity
✅ **Practical** - Examples over theory
✅ **Accurate** - Code verified, not assumed
✅ **Current** - Matches actual implementation

❌ No enterprise fluff
❌ No outdated information
❌ No assumptions without verification

## 5. Ask if Uncertain

If you're unsure about the intent behind a change or user-facing impact, **ask the user** - don't guess.
