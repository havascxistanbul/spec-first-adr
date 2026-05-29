# CLAUDE_LOG Format

`CLAUDE_LOG.md` lives at the repo root. It is an **append-only** ledger: newest entry at the bottom, never edit or reorder existing lines. Corrections are new lines that reference the earlier entry.

## Entry format

One entry per decision or meaningful action:

```
- [YYYY-MM-DD HH:MM] <TYPE> <one-line description> [-> ADR-NNNN | spec vX.Y]
```

`<TYPE>` is one of:

- `SPEC`   — spec locked, revised, or superseded
- `DECIDE` — a decision was made (link the ADR if it warranted one)
- `BUILD`  — implementation step completed
- `FIX`    — correction to a prior state (reference the line being corrected)
- `NOTE`   — context worth preserving that isn't a decision

## Rules

1. Append only. The value is the immutable sequence; rewriting it destroys the audit trail.
2. One line per entry. If it needs a paragraph, it needs an ADR — link to it instead.
3. Cross-link. If an entry has a backing ADR or spec revision, reference it on the same line.
4. Corrections reference, not replace. `FIX` lines point at what they correct; the original stays.

## Example

```markdown
# CLAUDE_LOG

- [2026-05-29 09:14] SPEC notification-service spec locked -> spec v1.0
- [2026-05-29 09:31] DECIDE chose Redis Streams over RabbitMQ for the queue -> ADR-0001
- [2026-05-29 09:48] DECIDE at-least-once delivery with idempotency keys -> ADR-0002
- [2026-05-29 10:22] BUILD producer + consumer skeleton wired, contract from spec section 5
- [2026-05-29 11:05] FIX corrected consumer ack timing from line 10:22; was acking before persistence
- [2026-05-29 14:40] SPEC spec revised to v1.1 — added dead-letter handling (was a deferred open question)
- [2026-05-29 14:41] DECIDE DLQ after 5 retries -> ADR-0003
```
