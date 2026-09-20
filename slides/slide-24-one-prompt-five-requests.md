# Slide 24: One prompt, five requests

### A “simple” request can still be a multi-step workflow

- One user request often expands into several model requests: decide what to inspect, interpret a result, propose or make an edit, read the test result, and revise if needed.
- This is normal agentic work. A multi-request loop is valuable when each request reduces uncertainty or verifies behavior.

### A healthy bug-fix loop

```text
Reproduce failure → read focused test → read implementation
	↓
Make smallest fix → run focused test → inspect result
	↓
Run relevant suite → inspect diff → report evidence
```

### Make every request earn its place

- Start with a specific symptom, file, or test so the first request is grounded.
- Prefer focused checks before broad suites; expand only when the risk or evidence requires it.
- Stop after the evidence proves the task is complete. Extra exploration after proof consumes time and context without increasing confidence.

### Example

"Fix the failing token-expiry test" may need five or more requests. That is efficient if it reproduces the failure, fixes the root cause, and returns a passing regression test instead of merely producing a plausible patch.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Context window](https://code.claude.com/docs/en/context-window)
- [Prompt caching](https://code.claude.com/docs/en/prompt-caching)
- [Claude Code costs](https://code.claude.com/docs/en/costs)

[← Previous slide](slide-23-the-round-trip-a-tool-call.md) · [Deck index](../README.md)