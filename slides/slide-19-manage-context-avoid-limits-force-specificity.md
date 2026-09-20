# Slide 19: Manage context: avoid limits, force specificity

### Specificity is the fastest way to reduce context overload

- This slide is about live-session habits. The fastest way to reduce unnecessary context is to narrow the next action before Claude starts exploring.

| Vague request                      | Specific request                                                                              |
| ---------------------------------- | --------------------------------------------------------------------------------------------- |
| "Scan the repo for the login bug." | "Read @src/auth/token.ts and @tests/auth/token.test.ts; reproduce the expiry failure."        |
| "Run the tests."                   | "Run `dotnet test --filter FullyQualifiedName~TokenExpiry` and show failing assertions only." |
| "Review our API."                  | "Review @src/controllers/OrdersController.cs for authorization and response-contract gaps."   |

### Tactics that preserve useful context

- Name the known files, symbols, error, and expected behavior. Add more context only when the investigation proves it is needed.
- Filter command output to failures or a focused test. Large logs remain part of the conversation and compete with decisions and evidence.
- Delegate broad exploration, log analysis, or documentation research to a subagent; receive a summary in the main session.
- Use `/compact` with an explicit preservation list for ongoing work. Use `/clear` before unrelated work, and `/rewind` instead of carrying a failed approach forward.

### Example

Replace "scan the repo" with: "Investigate the null reference in @src/services/InvoiceService.cs. Read the matching unit test, reproduce the failure, and return the root cause plus the smallest proposed fix." After a long investigation, compact the root cause, modified files, test command, and open decision before implementation continues.

### Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- [Permission modes](https://code.claude.com/docs/en/permission-modes)
- [Permissions](https://code.claude.com/docs/en/permissions)
- [Memory](https://code.claude.com/docs/en/memory)
- [Context window](https://code.claude.com/docs/en/context-window)

[← Previous slide](slide-18-context-engineering-what-should-load-when.md) · [Deck index](../README.md)
