# Worked Example: Notification Service

A compact end-to-end illustration of the discipline. This is what "good" looks like at small scale — read it to calibrate depth, then scale up or down to fit the real task.

---

## 1. The spec (locked)

```markdown
# Spec: Notification Service

- Version: 1.0
- Status: Locked
- Date: 2026-05-29

## 1. Problem
Users miss time-sensitive events because there is no central way to deliver
in-app and email notifications reliably.

## 2. Goals
- Deliver a notification to a user via in-app and/or email.
- Survive a consumer crash without losing notifications.
- Sub-second enqueue latency.

## 3. Non-goals
- SMS / push (future work).
- User notification preferences UI (separate service owns this).
- Templating engine (callers send rendered content).

## 4. Constraints
- Must use existing Redis cluster; no new infra.
- Existing auth service issues the user identity; do not reinvent.

## 5. Interfaces / Contracts
- Event: notification.requested { userId, channels[], subject, body }
- Event: notification.delivered { notificationId, channel, ts }
- HTTP: POST /notifications -> 202 Accepted { notificationId }

## 6. High-level approach
Enqueue on a durable stream, process with idempotent consumers per channel.

## 7. Open questions
| # | Question | Resolution | Owner |
| 1 | Retry policy on email failure? | Deferred to v1.1 | me |

## 8. Lock record
- Locked 2026-05-29.
```

Note the deferred open question — it is explicitly parked with an owner, not silently ignored. That is what lets the spec lock honestly.

## 2. The ADRs

```markdown
# ADR-0001: Use Redis Streams for the notification queue

- Status: Accepted
- Date: 2026-05-29

## Context
We need durable, ordered, at-least-once delivery. Constraint: reuse the
existing Redis cluster, no new infrastructure.

## Decision
We will use Redis Streams with consumer groups as the notification queue.

## Alternatives considered
- RabbitMQ: better routing, but new infra — violates the no-new-infra constraint.
- Postgres LISTEN/NOTIFY: not durable across restarts.
- Redis Streams: durable, ordered, already available. Chosen.

## Consequences
- Positive: zero new infra, durable, consumer groups give us scaling.
- Negative / accepted cost: weaker routing than RabbitMQ; we hand-roll DLQ later.
- Follow-up: retry/DLQ policy still open (see spec open question 1).
```

```markdown
# ADR-0002: At-least-once delivery with idempotency keys

- Status: Accepted
- Date: 2026-05-29

## Context
Redis Streams gives at-least-once, so consumers can see a message twice.
Sending a user the same email twice is a visible defect.

## Decision
Each notification carries an idempotency key; consumers record processed
keys and skip duplicates.

## Alternatives considered
- Exactly-once via distributed transaction: too complex for the constraint set.
- Accept duplicates: unacceptable user-visible defect.
- Idempotency keys: standard, cheap, sufficient. Chosen.

## Consequences
- Positive: duplicates become harmless.
- Negative / accepted cost: a small dedupe store to maintain and expire.
```

## 3. The CLAUDE_LOG

```markdown
# CLAUDE_LOG

- [2026-05-29 09:14] SPEC notification-service spec locked -> spec v1.0
- [2026-05-29 09:31] DECIDE chose Redis Streams over RabbitMQ -> ADR-0001
- [2026-05-29 09:48] DECIDE at-least-once + idempotency keys -> ADR-0002
- [2026-05-29 10:22] BUILD producer + consumer skeleton wired from spec section 5
```

## What to notice

- The spec locked **before** any BUILD line appears in the log.
- Each real decision got an ADR; the ADRs list rejected alternatives and accepted downsides.
- The log is terse and chronological; depth lives in the ADRs, sequence lives in the log.
- The deferred retry question is visible in three places (spec, ADR-0001 follow-up, and eventually a future log line) — nothing falls through a crack.
