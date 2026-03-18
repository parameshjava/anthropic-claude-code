# Chapter 10: Reduce Hallucinations and Unsafe Edits

## Grounding, Explicit Uncertainty, and Factual Evidence

---

The number one concern enterprises have about AI-assisted coding is: **"How do we know Claude is not making things up?"**

The answer is a six-step trust pipeline. Each step adds a layer of verification that catches hallucinations and unsafe edits before they reach production.

### The Trust Pipeline

#### Step 1: Ground

**What:** Use `@` file references to load specific modules. Add permanent grounding rules in CLAUDE.md.

**Why:** Grounding means giving Claude the actual source code to reason about — not asking it to recall from training data. When Claude reads your actual `handler.ts`, it reasons about your actual code. When it guesses, it hallucinates.

**Example CLAUDE.md rule:**
```markdown
## Grounding Rules
- Always read the relevant source files before proposing changes
- Never suggest code based on assumptions about file structure
- If a file reference is not provided, ask which files to read
```

#### Step 2: Plan

**What:** Use `/plan` to preview Claude's approach before it makes any edits.

**Why:** Plans catch wrong assumptions early. If Claude's plan says "I'll modify the session middleware to..." and you know the session middleware should not be touched, you catch it before any code changes.

**Example interaction:**
```
You: /plan Fix the duplicate row bug in paginated search
Claude: Here's my plan:
  1. Read @src/api/search.ts — understand the pagination logic
  2. Read @src/db/queries/pagination.ts — check offset calculation
  3. Hypothesis: off-by-one in OFFSET calculation
  4. Fix: adjust boundary condition
  5. Add regression test with edge case page boundaries
  6. Run search test suite

You: Plan looks good, but also check @src/api/middleware/cache.ts —
     we cache search results and the bug might be there.
```

The plan costs almost nothing. Catching a wrong approach after 10 file edits costs everything.

#### Step 3: Evidence

**What:** Require file paths, test output, and command results. Use Claude Code Security for vulnerability checks.

**Why:** "I fixed it" is not evidence. "Here is the diff, here is the test output showing 15/15 passing, and here is the security scan showing no new vulnerabilities" — that is evidence.

#### Step 4: Validate

**What:** Run tests, linters, or targeted commands and report results. Automate via hooks.

**Why:** Validation must be automated, not optional. Set up a PostToolUse hook that runs your linter after every edit:

```json
{
  "hooks": {
    "PostToolUse": [{
      "matcher": "Edit|Write",
      "command": "npx eslint --fix $FILEPATH"
    }]
  }
}
```

Now every edit is automatically linted — Claude cannot skip this step.

#### Step 5: Review

**What:** Summarize changed files, risks, and intentional non-changes before committing.

**Why:** The review step forces Claude to explicitly state what it changed and what it chose NOT to change. This surfaces both errors of commission (wrong changes) and omission (missing changes).

**Example prompt addition:**
```
Before committing, list:
1. Every file changed and why
2. Files you considered but chose not to change, and why
3. Any risks or edge cases in the changes
```

#### Step 6: Escalate

**What:** If Claude is unsure, it should say so and explain how a human should verify.

**Why:** The worst hallucination is a confident one. A well-calibrated Claude says "I'm 80% confident this is a race condition, but you should verify by running the load test at 100 concurrent requests." That uncertainty signal is invaluable.

### The Golden Rule

> **Never trust "looks right." Prefer file evidence, test output, and explicit uncertainty.**

Add hooks to auto-run linters and tests after edits. Make verification automatic, not aspirational.

---

## Anti-Patterns to Watch For

| Anti-Pattern | Better Approach |
|-------------|-----------------|
| "Claude said it's fixed, let's ship" | Run the tests, review the diff, check for regressions |
| Accepting code without file references | Require Claude to cite which files it read |
| Skipping plan mode for complex changes | Plan first, execute second |
| Ignoring "I'm not sure about..." | Treat uncertainty as a valuable signal, investigate |
