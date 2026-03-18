# Chapter 3: Which Model for Which Task?

## Sonnet by default. Opus for complexity. Haiku for speed.

---

This chapter maps the three most common engineering workflows to the right model — with clear escalation triggers and real-world example prompts.

### Debugging & Fixing

The **default** is Sonnet 4.6. Sonnet is excellent at reading stack traces, tracing data flows, and proposing focused fixes.

**Escalate to Opus 4.6** when the bug is deep, cross-cutting, intermittent, or architecture-related. If three developers have already looked at a bug and nobody can reproduce it — that is an Opus problem. Opus excels at reasoning across multiple files and understanding subtle interactions.

**Use Haiku 4.5** for fast triage — scanning logs, summarizing error patterns, running parallel investigations across modules. Haiku does not need to understand the full architecture; it just needs to find the needle in the haystack.

**Example prompt for debugging:**

```
Investigate duplicate rows in paginated search results.
Read @src/api/search.ts and @src/db/queries/pagination.ts.
Identify root cause, propose the smallest safe fix,
and add a regression test. Do not change the API response shape.
```

Notice the structure: goal, specific files, constraint, validation requirement. This prompt formula is covered in detail in later chapters.

### New Development

Again, **default to Sonnet 4.6**. Sonnet handles most feature work — new endpoints, UI components, database migrations, test suites.

**Escalate to Opus 4.6** when the feature is ambiguous ("we need better notifications"), multi-step (requires a migration, API change, and frontend update), or requires heavy cross-codebase planning.

**Use Haiku 4.5** for scaffolding boilerplate, generating test matrices, and running parallel subtasks. For instance, if 15 similar test files are needed, dispatch Haiku subagents to generate them in parallel.

**Example prompt for new development:**

```
Add soft delete for projects. Add a deleted_at column,
update the repository layer to filter soft-deleted records,
preserve the existing response shape, follow the repo pattern
in @src/db/migrations/, and add migration + tests.
```

### Design & Architecture

This is where Opus 4.6 shines. **Escalate to Opus** for trade-off analysis, multi-document synthesis, and strategic architecture decisions.

Sonnet is still the default for straightforward design questions ("should we use Redis or Memcached for this cache?"), but when evaluating three competing approaches and mapping implications across the full stack — use Opus.

**Use Haiku 4.5** for chunking long documents, summarizing specs, and extracting structured fields from design docs.

**Example prompt for design:**

```
Propose 3 workflow editor designs for our approval system.
Map trade-offs (complexity, latency, maintainability),
recommend one, and outline backend implications.
Reference @docs/requirements.md and @src/workflows/.
```

---

## Key Takeaway

The model selection is not a one-time decision — it is a per-task decision. Train your engineers to ask: "Is this a Haiku task, a Sonnet task, or an Opus task?" before they start typing.
