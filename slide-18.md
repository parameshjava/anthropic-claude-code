# Chapter 18: Claude Code Security — Reasoning-Based Vulnerability Scanning

## Uses Claude's Reasoning to Find Zero-Day Vulnerabilities That Traditional Tools Miss

---

Claude Code Security, launched in February 2026, represents a fundamental shift in application security scanning.

Traditional SAST and DAST tools work by **pattern matching** — they have a database of known vulnerability patterns and check code against them. This works for known CVEs but completely misses **logic bugs, business logic vulnerabilities, and zero-day exploits** that do not match any pattern.

Claude Code Security uses **reasoning-based analysis**. Claude reads code the way a senior security engineer would — understanding data flows, authentication patterns, business logic, and how components interact. It then reasons about how those patterns could be exploited.

### How It Works: Four Steps

#### Step 1: Deep Code Analysis

Claude reads and reasons about the **entire codebase**. Not just individual files — it understands how data flows from user input through API handlers, into database queries, and back through response serializers.

For example, Claude might trace: "User input in `req.body.email` flows through `validateUser()` to `db.query()` — but `validateUser()` only checks format, not injection. The database query uses string interpolation, not parameterized queries."

#### Step 2: Vulnerability Detection

Using reasoning — not pattern matching — Claude identifies vulnerabilities. This includes:
- SQL injection through unexpected input paths
- Authentication bypasses in complex middleware chains
- Authorization failures where role checks are inconsistent
- Race conditions in concurrent operations
- Data leakage through error messages or logs
- Business logic flaws (e.g., "users can modify prices by manipulating the cart API")

#### Step 3: Contextual Assessment

Claude evaluates severity with **full understanding of the specific codebase**. A SQL injection in an internal admin tool is different from one in a public-facing API. Claude understands this context and prioritizes accordingly.

#### Step 4: Fix Recommendations

This is where Claude Code Security is fundamentally different from traditional tools. Instead of flagging a line number and saying "possible SQL injection," Claude generates:
- **Concrete code changes** — the actual fix, not just a description
- **Explanation of the vulnerability** — why it exists and how it could be exploited
- **Impact assessment** — what an attacker could do with this vulnerability
- **Test suggestions** — how to verify the fix works

### Reasoning vs. Pattern Matching

| Aspect | Traditional SAST | Claude Code Security |
|--------|-----------------|---------------------|
| Detection method | Pattern-based matching | Reasoning-based analysis |
| False positive rate | High — flags "possible" issues | Context-aware — understands if risk is real |
| Logic bugs | Cannot find (no pattern exists) | Finds by reasoning about data flow |
| Zero-day vulnerabilities | Misses until rule is updated | Reasons about novel attack vectors |
| Data flow understanding | Limited to single-file analysis | Cross-file, cross-module understanding |
| Output quality | Flagged lines, no fixes | Concrete fix code + explanation |

### Integration Options

Claude Code Security fits into existing workflows at multiple points:

**CLI:** Run on demand from the terminal:
```bash
claude security review
```

**GitHub Action:** Auto-scan every PR before merge. Add it to the CI/CD pipeline so no vulnerable code ships without a security review.

**Hook:** Configure as a PreToolUse gate. Before Claude edits any file, the security scanner checks for vulnerabilities introduced by the change.

**API:** Integrate into an existing AppSec toolchain. Feed results into a vulnerability management system alongside existing SAST/DAST results.

### The Shift-Left Security Pipeline

Combining Claude Code Security with hooks and GitHub Actions creates a complete **shift-left security pipeline**:

1. **Code time** — Hooks scan changes as Claude writes them
2. **PR time** — GitHub Action scans the full diff
3. **Pre-merge** — Security gate requires clean scan before merge
4. **Post-deploy** — Scheduled scans of the full codebase

Security is not a gate at the end — it is embedded at every stage from code to deploy.

---

## Example: Finding What SAST Misses

**Scenario:** A SAST tool reports zero vulnerabilities. Claude Code Security finds:

```
CRITICAL: Authorization bypass in /api/projects/:id/settings

The endpoint checks `req.user.role === 'admin'` but the project
membership check uses `project.members.includes(req.user.id)`.
An admin of Project A can modify settings of Project B because
the role check is global, not project-scoped.

Fix: Replace role check with project-scoped permission:
  - const isAdmin = req.user.role === 'admin'
  + const isAdmin = project.admins.includes(req.user.id)

Impact: Any organization admin can modify any project's settings,
including projects they are not a member of.
```

No SAST tool would find this because there is no "pattern" for project-scoped authorization logic. Claude found it by reasoning about the business logic.
