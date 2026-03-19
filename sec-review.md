---
description: Automated code and security review checklist for React and Next.js applications
skills:
  - code-quality
  - security-review
  - owasp-security
---

# Code Review Task

Perform a comprehensive code quality review using the `code-quality` skill including security review using the `security-review` skill and `owasp-security` skill. Be thorough but concise.

## 1. Code Quality Checks

### Logging ✅❌
- [ ] No `console.log/error/warn/info/debug` statements in production code.
- [ ] Uses centralized `logger` (e.g., from `@/lib/logger/`).
- [ ] Logger calls include relevant context objects for tracing.

### Error Handling ✅❌
- [ ] Try-catch blocks are present for async operations and external API calls.
- [ ] Errors use centralized handlers (e.g., from `@/lib/errors`).
- [ ] User-facing error messages are sanitized and helpful.
- [ ] Critical errors are logged with stack traces and context.

### TypeScript ✅❌
- [ ] No `any` types (unless absolutely necessary and explicitly commented).
- [ ] Interfaces/types are defined for complex objects and API responses.
- [ ] No type assertions (`as Type`) without justification.
- [ ] No ignored TypeScript errors (`@ts-ignore` or `@ts-expect-error` without explanations).

### Production Readiness ✅❌
- [ ] No debug statements, temporary test data, or version markers.
- [ ] No incomplete TODOs (resolve them or track them in a ticket).
- [ ] No hardcoded secrets, tokens, or absolute local paths.
- [ ] No commented-out code blocks left behind.

## 2. React & Next.js Checks

### Component Architecture (App Router) ✅❌
- [ ] Proper separation of Server Components (default) and Client Components (`"use client"`).
- [ ] `"use client"` is pushed down the tree as far as possible to maximize server rendering.
- [ ] Route handlers (`route.ts`) and layouts (`layout.tsx`) follow Next.js file conventions.
- [ ] UI components are decoupled from business logic and data fetching where appropriate.

### Hooks & State Management ✅❌
- [ ] Effects (`useEffect`) have proper cleanup functions to prevent memory leaks.
- [ ] Hook dependency arrays are complete and accurate.
- [ ] No infinite rendering loops triggered by state updates during render.
- [ ] Uses appropriate state boundaries; avoids heavy global state for localized UI logic.

### Performance & Optimization ✅❌
- [ ] Uses Next.js `<Image />` component instead of native `<img>` for automatic optimization.
- [ ] Uses Next.js `<Link />` component instead of native `<a>` tags for client-side navigation.
- [ ] Expensive calculations are memoized (`useMemo`), and stable callbacks use `useCallback`.
- [ ] Third-party scripts utilize the Next.js `<Script />` component with appropriate loading strategies.

### Security (Next.js Specific) ✅❌
- [ ] Server Actions validate inputs (e.g., using Zod) before processing mutations.
- [ ] Server Actions verify user authentication and authorization (RLS policies/permissions).
- [ ] No sensitive environment variables leak to the browser (must use `NEXT_PUBLIC_` only for client-safe vars).
- [ ] API routes and Server Actions have appropriate rate limiting or CSRF protection where necessary.

## Output Format

### ✅ Looks Good
- [Item 1]
- [Item 2]

### ⚠️ Issues Found
- **[Severity]** [`File:line`] - [Issue description]
  - **Fix:** [Suggested fix or refactor]
  - **Skill Reference:** [Note if a specific skill flagged this, e.g., `security-review`]

### 📊 Summary
- Files reviewed: [X]
- Critical issues: [X]
- Warnings: [X]

## Severity Levels
- **CRITICAL** - Security vulnerabilities, data loss, application crashes, sensitive data exposure.
- **HIGH** - Functional bugs, severe performance bottlenecks, major UX degradation.
- **MEDIUM** - Code quality violations, maintainability issues, minor TypeScript errors.
- **LOW** - Styling inconsistencies, naming conventions, minor optimization suggestions.