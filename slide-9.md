# Chapter 9: Prompt Formula for Claude Code

## A Good Prompt Gives Claude a Target, a Boundary, and a Way to Prove the Outcome

---

Every engineer using Claude Code should internalize the **prompt formula** — a six-part structure that turns vague requests into precise engineering specifications.

### The Six Parts

The following walkthrough uses a real example — fixing a JWT authentication bug:

#### 1. Goal
```
Fix the user auth bug by aligning claims in JWT token
as per RFC-7519: https://datatracker.ietf.org/doc/html/rfc7519
```
State what you want achieved. Be specific about the outcome, not just the symptom. Notice the RFC reference — Claude can read it and ground its work in the actual specification.

#### 2. Files
```
@src/auth/handler.ts @src/api/session.ts
```
Pin the exact files Claude should read. This prevents scanning and focuses all reasoning on the relevant code.

#### 3. Constraints
```
Do not change token format or middleware
```
What should Claude **not** touch? Constraints prevent scope creep. Without this, Claude might "helpfully" refactor the middleware while fixing the bug.

#### 4. Scope
```
Do not scan the full project — only read listed files
Do not scan entire RFC — scan only section 4:
https://datatracker.ietf.org/doc/html/rfc7519#section-4
```
Scope controls what Claude is allowed to explore. This is different from constraints (what not to change) — scope is about what not to **read**. This saves context and prevents Claude from going down rabbit holes.

#### 5. Validation
```
Add a regression test, run auth suite
```
How will you **prove** the fix works? This is non-negotiable. Every prompt should include a validation step. "Run the tests" is the minimum. Better: "Add a regression test that would have caught this bug, then run the full auth suite."

#### 6. Output
```
Root cause first, then smallest safe fix
```
Tell Claude how to structure its response. "Root cause first" means Claude explains **why** the bug exists before showing the fix. This helps you verify the reasoning, not just the code.

### The Complete Prompt

Putting it all together:

```
Fix the user auth bug by aligning claims in JWT token
as per RFC-7519 section 4.

Read @src/auth/handler.ts and @src/api/session.ts.
Do not change token format or middleware.
Do not scan the full project — only read listed files.

Add a regression test and run the auth suite.
Report root cause first, then the smallest safe fix.
```

That is 7 lines. It takes 30 seconds to write. And it produces dramatically better results than "fix the auth bug."

### The Prompt Skeleton Quick Reference

| Letter | Principle | Example |
|--------|-----------|---------|
| **A** | Plan first | Use `/plan` to map approach before writing code |
| **B** | Pin your files | `@` references keep context focused |
| **C** | Demand proof | Tests, lint, or type-check must pass |
| **D** | Guard scope | "Do not scan" saves tokens and prevents drift |
| **E** | Reuse patterns | Store reusable prompts in skills or CLAUDE.md |
| **F** | Batch smartly | Subagents and `/batch` for multi-file operations |

### When to Use Plan Mode

**Use `/plan`** when:
- The change touches multiple files
- You are not sure about the right approach
- The task has architectural implications

**Skip the plan** when:
- It is an obvious, small edit
- You are fixing a typo or renaming a variable
- The path is clear and well-constrained

### Prompt Best Practices Checklist

Use this checklist alongside the six-part formula. Not every prompt needs every item — pick the ones relevant to the task.

#### Ask, Don't Assume
- The single most effective way to reduce hallucinations: tell Claude to stop and ask when information is missing or requirements are unclear
- Without this, Claude fills gaps with plausible-sounding but potentially wrong guesses — invented API signatures, assumed column names, made-up config values
- Example line in prompt: "If any requirement is ambiguous or you cannot find the needed information in the listed files, stop and ask before proceeding. Do not guess."
- For implementation tasks: "If a function signature, type definition, or config value is not visible in the listed files, ask me for it rather than assuming what it looks like."
- Add to CLAUDE.md for project-wide enforcement: "When requirements are unclear or necessary context is missing, always ask for clarification before writing code. Never invent interfaces, schemas, or behaviors not explicitly defined in the codebase."

#### Error Handling & Edge Cases
- Specify how errors should be handled (return error response, throw, log and continue?)
- Ask Claude to identify and test edge cases (null inputs, empty arrays, max boundaries, concurrent access)
- Example line in prompt: "Handle cases where reportId does not exist (return 404), date_from is after date_to (return 400 with message), and columns contains unknown names (ignore unknown, process valid ones)"

#### Impact Analysis
- Ask Claude to identify what else the change could affect before writing code
- Example line: "Before making changes, list all files that import from productRepository.ts and confirm none will break"
- For shared types/interfaces: "Check all consumers of the Project type and verify the change is backward compatible"

#### Use /plan for Complex Changes
- Start multi-file changes with /plan to preview the approach
- Example workflow:
  /plan → review Claude's approach → adjust if needed → approve → execute
- Add to prompt: "Use /plan first. Do not write code until I approve the plan."

#### Incremental Verification
- For multi-file changes, verify after each file rather than all at once
- Example line: "Make changes incrementally: update the repository first, run its tests, then update the service layer, run its tests, then update the API handler. Stop if any step fails."
- This catches issues early and keeps context clean

#### Logging & Observability
- Specify whether the change needs structured logging for production debugging
- Example line: "Add structured log entries (using the existing logger from @src/shared/logger.ts) for: successful export start, row count, export completion time, and any errors. Use log level info for success, error for failures."
- For performance changes: "Add timing metrics to measure query execution time before and after the fix"

#### Rollback Strategy
- Specify how to undo the change if something goes wrong in production
- Example line: "Ensure the migration has a working down() function that cleanly reverts the schema change. Test rollback: run npm run migrate:rollback and verify the previous state is restored."
- For feature flags: "The flag OFF state must be identical to the current behavior — this is the rollback path"

#### Security Review Trigger
- For changes touching auth, payments, PII, or external APIs, flag for human security review
- Example line: "This change touches authentication logic. After implementation, run /security-review and flag any findings. List all input paths that could be exploited."
- Add to CLAUDE.md: "Any change to src/auth/, src/payments/, or src/middleware/session.ts must include a security review summary in the output"

#### Breaking Change Check
- Ask Claude to verify no downstream consumers are broken
- Example line: "Check all files that import from projectRepository.ts. Verify that the new deleted_at filter does not break any existing caller. List any callers that may need the includeDeleted option."
- For API changes: "Verify the response shape is unchanged. Compare the TypeScript type before and after. List any client-facing differences."

---

## Real-World Prompt Examples

The following five examples demonstrate the prompt formula applied to common enterprise engineering tasks. Each follows the structure: Goal, Files, Constraints, Scope, Validation, and Output.

---

### Example 1: Fixing a Database Query Performance Issue

**Bad prompt:** "The search API is slow, can you fix it?"

**Good prompt:**

```markdown
# Goal
The GET /api/products/search endpoint returns results in 4-8 seconds
for queries with more than 3 filter parameters. Target response time
is under 500ms. The slow query log shows a sequential scan on the
products table when category, price_range, and availability filters
are combined.

# Files
Read @src/api/products.ts for the endpoint handler.
Read @src/repositories/productRepository.ts for the query builder.
Read @src/db/migrations/20250812_create_products.ts for the current index definitions.

# Constraints
- Do not change the API response shape or query parameter contract
- Do not drop and recreate existing indexes — only add new ones
- Do not modify the Knex query builder abstraction layer
- Keep backward compatibility with existing clients

# Scope
- Only read the listed files and @src/db/connection.ts for pool config
- Do not scan the full src/db/ directory

# Validation
- Run EXPLAIN ANALYZE on the problematic query before and after the fix
- Add a new composite index migration if needed
- Run only the scoped test file: npm test -- src/repositories/__tests__/productRepository.test.ts
- Add a performance regression test that fails if the query exceeds 500ms
- Run scoped coverage: npm test -- --coverage src/repositories/__tests__/productRepository.test.ts
- Coverage on productRepository.ts must not drop below the current threshold
- Delegate full suite regression check to a subagent:
  spawn a subagent to run "npm test" and report only failures (keep verbose
  output out of main context)

# Output
- Root cause analysis first (why the sequential scan happens)
- Proposed index strategy with trade-off analysis (write overhead vs. read speed)
- The migration file for any new indexes
- Before/after EXPLAIN ANALYZE output
```

---

### Example 2: Implementing a New REST API Endpoint with Validation

**Bad prompt:** "Add an endpoint for exporting reports as CSV"

**Good prompt:**

```markdown
# Goal
Add a new GET /api/reports/:reportId/export endpoint that returns
report data as a downloadable CSV file. The endpoint must support
optional query parameters: date_from, date_to (ISO 8601), and columns
(comma-separated list of column names to include). Reports can have
up to 500K rows — the response must stream to avoid memory issues.

# Files
Read @src/api/reports.ts for existing report endpoints and patterns.
Read @src/services/reportService.ts for the report data retrieval logic.
Read @src/shared/types/report.ts for the Report type definition.
Read @src/middleware/validate.ts for the Zod validation pattern.

# Constraints
- Follow the existing endpoint pattern in @src/api/reports.ts
- Use Zod schema for query parameter validation (see validate middleware)
- Stream the CSV response using Node.js Transform streams — do not buffer
  the full dataset in memory
- Use the { data, error, meta } response shape for error cases only —
  success streams raw CSV with Content-Type: text/csv
- Do not add new npm dependencies — use built-in Node.js streams

# Scope
- Only read the listed files
- Do not scan the full src/api/ directory
- Do not modify existing endpoints in reports.ts — only add the new one

# Validation
- Read existing tests in @src/api/__tests__/reports.test.ts before
  writing new tests — follow the same patterns and helpers
- Add Zod schema validation tests for all query parameter edge cases
  (invalid dates, unknown columns, missing reportId)
- Add integration test with a mock dataset of 1000 rows
- Add test for streaming behavior (memory should not spike with large datasets)
- Run only the scoped test files:
  npm test -- src/api/__tests__/reports.test.ts src/api/__tests__/reportExport.test.ts
- Run scoped coverage: npm test -- --coverage src/api/__tests__/reportExport.test.ts
- New endpoint must have minimum 90% line and branch coverage
- Delegate full suite regression check to a subagent:
  spawn a subagent to run "npm test" and report only failures
- Run: npm run typecheck

# Output
- Implementation plan first (which files change and why)
- Zod validation schema
- Endpoint handler with streaming logic
- Test file with all test cases
```

---

### Example 3: Debugging a Race Condition in Async Processing

**Bad prompt:** "Users are seeing duplicate order confirmation emails"

**Good prompt:**

```markdown
# Goal
Customers report receiving 2-3 duplicate order confirmation emails
for a single purchase. The issue occurs under load (~50+ concurrent
orders per minute) and is not reproducible with single requests.
The likely cause is a race condition in the order event handler —
the "order.confirmed" event fires before the idempotency check
in the email service completes.

# Files
Read @src/events/orderEventHandler.ts for the event listener.
Read @src/services/emailService.ts for the email sending logic
and the idempotency check.
Read @src/jobs/emailQueue.ts for the BullMQ job configuration.
Read @src/db/repositories/emailLogRepository.ts for the sent-email
deduplication logic.

# Constraints
- Do not change the BullMQ queue configuration or job retry settings
- Do not add distributed locks unless no simpler solution exists —
  prefer database-level uniqueness constraints
- Do not modify the order event publishing logic in @src/services/orderService.ts
- Preserve existing email templates and content

# Scope
- Only read the listed files
- Do not scan the full src/events/ or src/jobs/ directories
- Check @src/db/migrations/ only if a new migration is needed

# Validation
- Read existing tests in @src/services/__tests__/emailService.test.ts
  and @src/events/__tests__/orderEventHandler.test.ts before making changes
- Root cause must be confirmed with a reproducible test:
  simulate 10 concurrent "order.confirmed" events for the same orderId
  and verify exactly 1 email is sent
- Add a concurrency-specific regression test that fires 10 parallel
  events for the same orderId and asserts exactly 1 email is sent
- Run only the scoped test files:
  npm test -- src/services/__tests__/emailService.test.ts src/events/__tests__/orderEventHandler.test.ts
- Run scoped coverage:
  npm test -- --coverage src/services/__tests__/emailService.test.ts src/events/__tests__/orderEventHandler.test.ts
- Coverage on emailService.ts and orderEventHandler.ts must not decrease
  from the current baseline
- Delegate full suite regression check to a subagent:
  spawn a subagent to run "npm test" and return only pass/fail summary
  with failure details (keep full output out of main context)

# Output
- Root cause analysis: explain exactly where the race condition occurs
  and why the current idempotency check fails under concurrency
- Sequence diagram (text-based) showing the race window
- Smallest safe fix with explanation of why it eliminates the race
- Diff of all changed files
```

---

### Example 4: Adding Feature Flag Support to an Existing Feature

**Bad prompt:** "Add a feature flag for the new dashboard"

**Good prompt:**

```markdown
# Goal
Wrap the new analytics dashboard (shipped in PR #347) behind a
LaunchDarkly feature flag so it can be gradually rolled out.
When the flag "analytics-dashboard-v2" is OFF, users should see
the existing dashboard at /dashboard. When ON, they see the new
dashboard at the same route. The flag should support percentage-based
rollout and targeting by user role (admin vs. standard).

# Files
Read @src/frontend/pages/Dashboard.tsx for the current dashboard component.
Read @src/frontend/pages/DashboardV2.tsx for the new dashboard component.
Read @src/frontend/hooks/useFeatureFlag.ts for the existing LaunchDarkly hook.
Read @src/shared/flags.ts for the feature flag registry and types.

# Constraints
- Use the existing useFeatureFlag() hook — do not create a new one
- Register the flag in @src/shared/flags.ts following the existing pattern
- Do not modify DashboardV2.tsx or Dashboard.tsx internals —
  only add the conditional rendering wrapper
- The fallback (flag OFF or LaunchDarkly unavailable) must always
  show the old dashboard — fail safe, not fail open
- Do not remove the old Dashboard component — it stays until the
  flag is fully rolled out and removed

# Scope
- Only read the listed files
- Do not scan the full src/frontend/ directory
- Do not modify any backend code — this is frontend-only

# Validation
- Read existing tests in @src/frontend/__tests__/Dashboard.test.tsx
  to understand the current test patterns and coverage
- Add tests for both flag states: ON shows V2, OFF shows V1
- Add test for LaunchDarkly SDK failure/timeout — must fall back to V1
- Add test for role-based targeting (admin sees V2, standard sees V1
  when configured)
- Run only the scoped test file:
  npm test -- src/frontend/__tests__/Dashboard.test.tsx
- Run scoped coverage:
  npm test -- --coverage src/frontend/__tests__/Dashboard.test.tsx
- The new conditional wrapper must be fully covered (100% branch coverage
  on the flag check — both ON and OFF paths)
- Delegate full frontend suite check to a subagent:
  spawn a subagent to run "npm test -- src/frontend/" and report
  only failures (keep verbose output out of main context)
- Run: npm run typecheck

# Output
- List of all files changed with one-line explanation per file
- The flag registration in flags.ts
- The conditional rendering implementation
- All test cases
```

---

### Example 5: Database Migration for a Schema Change with Zero Downtime

**Bad prompt:** "Add soft delete to the projects table"

**Good prompt:**

```markdown
# Goal
Add soft delete support to the projects table. Projects should not
be physically deleted — instead, a deleted_at timestamp column marks
them as deleted. All existing queries must filter out soft-deleted
records by default. The migration must be zero-downtime compatible
(no table locks on the 2M+ row projects table in production).

# Files
Read @src/db/migrations/ directory listing to find the latest migration number.
Read @src/repositories/projectRepository.ts for all current query methods.
Read @src/services/projectService.ts for the delete logic.
Read @src/api/projects.ts for the DELETE endpoint handler.
Read @src/shared/types/project.ts for the Project type definition.

# Constraints
- Migration must be zero-downtime: use ALTER TABLE ... ADD COLUMN with
  a DEFAULT value (Postgres adds nullable columns without a table lock)
- Do not modify the API response shape — soft-deleted projects must
  simply stop appearing in list/get responses
- Do not change the DELETE endpoint URL or method — it should still be
  DELETE /api/projects/:id, but now it sets deleted_at instead of
  physically deleting
- Preserve existing foreign key relationships — do not cascade soft deletes
  to related tables (tasks, members) in this migration
- Add an explicit includeDeleted option to repository methods for
  admin use cases

# Scope
- Only read the listed files
- Do not scan the full src/ directory
- Do not modify task or member repositories — those will be updated
  in a follow-up ticket

# Validation
- Read existing tests in @src/repositories/__tests__/projectRepository.test.ts
  and @src/api/__tests__/projects.test.ts before writing new tests
- Migration must run successfully against a copy of production schema
- Run: npm run migrate (verify migration applies cleanly)
- Run: npm run migrate:rollback (verify rollback works)
- Add new tests:
  - DELETE sets deleted_at, does not remove the row
  - GET /api/projects excludes soft-deleted records
  - GET /api/projects with includeDeleted=true returns all records
  - Unique constraints still work (cannot create a project with the
    same name as a soft-deleted one — or can? specify the decision)
- Run only the scoped test files:
  npm test -- src/repositories/__tests__/projectRepository.test.ts src/api/__tests__/projects.test.ts
- Run scoped coverage:
  npm test -- --coverage src/repositories/__tests__/projectRepository.test.ts src/api/__tests__/projects.test.ts
- New soft delete methods must have minimum 90% line and branch coverage
- Coverage on projectRepository.ts and projectService.ts must not drop
  below current baseline
- Delegate full suite regression check to a subagent:
  spawn a subagent to run "npm test" and return only pass/fail count
  with failure details (keep 1000+ test output out of main context)
- Run: npm run typecheck

# Output
- Migration file with up() and down() functions
- Repository changes with before/after diff
- Service layer changes
- API handler changes
- Complete test file
- Rollback verification output
```

---

Each example follows the same formula: **Goal** (what and why), **Files** (exactly where to look), **Constraints** (what not to touch), **Scope** (what not to scan), **Validation** (how to prove it works), and **Output** (how to structure the response). Adapt this pattern to any engineering task.
