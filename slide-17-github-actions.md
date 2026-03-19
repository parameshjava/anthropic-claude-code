# Chapter 17: GitHub Actions — Claude in Your CI/CD Pipeline

## Automated PR Reviews, Test Generation, and Issue-to-PR Workflows

---

Claude Code is not limited to the terminal. With the official **claude-code-action**, Claude runs directly in a GitHub CI/CD pipeline — reviewing PRs, generating tests, fixing issues, and automating implementations.

### The Four Core Workflows

#### 1. Automated PR Review

When a pull request is opened, Claude automatically:
- Analyzes the diff
- Posts inline comments on specific lines
- Flags potential issues — bugs, performance problems, security concerns
- Suggests improvements

```yaml
on:
  pull_request:
    types: [opened, synchronize]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: anthropics/claude-code-action@v1
        with:
          prompt: "Review this PR for bugs, security issues, and style violations"
```

This runs on every PR automatically. No engineer needs to remember to request a review.

#### 2. @claude Mentions

Tag `@claude` in any issue or PR comment to get on-demand help:

```
@claude This endpoint is throwing 500 errors under load.
Can you investigate the connection pooling in src/db/pool.ts?
```

Claude reads the relevant code, investigates, and posts its analysis as a comment. This turns GitHub issues into an interactive debugging environment.

#### 3. Test Generation

Comment `/gen-tests` on any PR, and Claude:
- Reads the changed files
- Generates comprehensive tests
- Creates a new branch with the tests
- Posts a coverage report as a PR comment

This is particularly powerful for teams with test coverage gaps. Claude can generate tests for existing untested code as part of the PR review process.

#### 4. Issue-to-PR Automation

Describe a feature in a GitHub issue, and Claude creates a full PR with the implementation:

```
Title: Add rate limiting to /api/upload
Body: Max 10 requests per minute per API key.
      Use Redis for the counter. Return 429 with Retry-After header.
```

Claude reads the issue, understands the codebase, implements the feature, writes tests, and opens a PR — all automatically. A human reviews and merges.

### Setup: Three Steps

1. **Install the Claude GitHub App** on your repository
2. **Add `ANTHROPIC_API_KEY`** to your repository secrets
3. **Copy the workflow YAML** to `.github/workflows/`

That is it. Three steps and you have AI-powered CI/CD.

**Enterprise deployment:** Use AWS Bedrock or Vertex AI instead of direct API keys for data residency compliance.

### Security Best Practices

These are non-negotiable for enterprise adoption:

| Practice                                  | Why                                                            |
| ----------------------------------------- | -------------------------------------------------------------- |
| **Never hardcode API keys**               | Use GitHub Secrets exclusively                                 |
| **Scope permissions to least privilege**  | Each job gets only the permissions it needs                    |
| **Pin actions by commit SHA**             | Prevents supply-chain attacks via compromised action versions  |
| **Require human approval before merge**   | Claude proposes, humans approve — always                       |
| **Fork PRs don't get secrets by default** | Prevents malicious forks from accessing your API keys          |
| **Use `--max-turns` to cap cost per run** | Prevents runaway sessions from consuming excessive API credits |

### Built on Claude Agent SDK

The GitHub Action is built on the **Claude Agent SDK**, which means it can be extended beyond GitHub. Build custom automation for any CI/CD platform — GitLab, Bitbucket, Jenkins, or an internal build system — using the same SDK.

---

## Real-World Example: Full CI/CD Integration

```yaml
name: Claude CI/CD
on:
  pull_request:
    types: [opened, synchronize]
  issue_comment:
    types: [created]

jobs:
  auto-review:
    if: github.event_name == 'pull_request'
    runs-on: ubuntu-latest
    steps:
      - uses: anthropics/claude-code-action@v1
        with:
          prompt: |
            Review this PR:
            1. Check for bugs and security issues
            2. Verify test coverage for changed functions
            3. Flag any breaking API changes
            4. Suggest performance improvements

  claude-mention:
    if: contains(github.event.comment.body, '@claude')
    runs-on: ubuntu-latest
    steps:
      - uses: anthropics/claude-code-action@v1
        with:
          prompt: "${{ github.event.comment.body }}"
          max_turns: 10
```

Every PR gets reviewed. Every `@claude` mention gets a response. All automated. All auditable.

---

## Useful Links

- [Claude Code GitHub Action](https://github.com/anthropics/claude-code-action)
- [GitHub Action Setup Guide](https://github.com/anthropics/claude-code-action/blob/main/docs/setup.md)
- [GitHub Action Usage Guide](https://github.com/anthropics/claude-code-action/blob/main/docs/usage.md)
- [Claude Code Base Action](https://github.com/anthropics/claude-code-base-action)
- [Claude Code Security Review Action](https://github.com/anthropics/claude-code-security-review)
- [Claude Code — GitHub Repository](https://github.com/anthropics/claude-code)
