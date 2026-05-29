---
name: spec-first-adr
description: Enforce a spec-first engineering discipline. Before any non-trivial implementation, lock a spec, then record an Architecture Decision Record (ADR) for every significant choice, and maintain an append-only CLAUDE_LOG of decisions across the session. Use this skill whenever the user starts a new feature, service, or refactor; mentions "spec", "ADR", "architecture decision", "design doc", "decision log", "lock the spec", or "spec-first"; asks how to structure a build before coding; or wants Claude to stop coding-first and instead think, decide, and document before writing code. Trigger even when the user does not explicitly say "ADR" but is clearly about to make an architectural choice that future readers will need explained.
---

# Spec-First ADR

This skill installs a working discipline: **decide before you build, and write down why.**

The failure mode it prevents is the most common one in AI-assisted engineering — jumping straight to code, accumulating undocumented decisions, and producing a system nobody (including the author six weeks later) can reason about. This skill front-loads the thinking and leaves a durable trail.

There are three artifacts, produced in order:

1. **The Spec** — what we are building and the boundaries, locked before code.
2. **The ADR(s)** — one record per significant decision, with the alternatives and the reasoning.
3. **The CLAUDE_LOG** — an append-only, chronological ledger of decisions made during the session.

## When to apply this

Apply the full discipline for: new services, new features with non-obvious design, refactors that change a contract or boundary, anything multi-step, and anything where a reasonable engineer would later ask "why was it done this way?"

Skip the heavy machinery for trivial, reversible, single-file changes (a typo fix, a copy tweak, a one-line config). For those, a CLAUDE_LOG line is enough; no spec, no ADR. Judge by reversibility and blast radius, not by line count.

If in doubt, write the ADR. A cheap ADR beats an expensive archaeology session.

## The workflow

### Step 1 — Lock the spec FIRST

Before writing any implementation code, produce a spec and get explicit sign-off. Do not start coding while the spec is still open. The point of locking is that downstream work has a fixed target; a moving spec produces churn.

Use the template at `references/spec-template.md`. Read it and fill every section. A spec is "locked" only when:

- Scope and explicit non-goals are written down (non-goals matter as much as goals).
- Interfaces/contracts are specified (API shape, data model, events — whatever applies).
- Open questions are either resolved or explicitly deferred with an owner.
- The user has said some form of "locked" / "proceed" / "go".

State the lock clearly in the conversation, e.g. "Spec locked. Proceeding to implementation." After lock, scope changes are handled as a new spec revision (bump the version, note what changed), not silent edits — silent scope creep is exactly what this discipline exists to stop.

### Step 2 — Write an ADR for each significant decision

Whenever the build forces a real choice — a database, a framework, a sync-vs-async boundary, a consistency tradeoff, a build-vs-buy — stop and record it as an ADR before committing to it in code. Use `references/adr-template.md`.

Number ADRs sequentially and immutably: `ADR-0001`, `ADR-0002`, ... Once an ADR has status `Accepted`, never rewrite its body. If a later decision overturns it, write a NEW ADR with status `Accepted` and set the old one's status to `Superseded by ADR-XXXX`. The chain of superseded records IS the architectural history; destroying it destroys the value.

A good ADR is short and honest. The "Consequences" section must include the downsides you are accepting, not just the upsides. An ADR with no listed downside is a red flag that the decision wasn't actually examined.

ADR statuses: `Proposed` → `Accepted` → (optionally) `Superseded` or `Deprecated`.

### Step 3 — Maintain the CLAUDE_LOG

The CLAUDE_LOG is an append-only, human-readable ledger of decisions and meaningful actions taken during the session. It is lighter than an ADR: one line per entry, chronological, never edited or reordered. Where ADRs capture *architectural* decisions in depth, the CLAUDE_LOG captures the *sequence* — including small calls that don't warrant a full ADR but that a reviewer would want to see.

Append to `CLAUDE_LOG.md` at the repo root (create it if absent). Use the format in `references/claude-log-format.md`. One entry per decision/action, newest at the bottom. Never rewrite history; corrections are new lines that reference the earlier one.

Cross-link: when an entry corresponds to an ADR, reference it (`-> ADR-0003`). When the spec is locked or revised, log that too.

## Ordering rule

The order is not negotiable and is the whole point: **Spec lock → ADR per decision → CLAUDE_LOG throughout.** If you find yourself writing code before the spec is locked, stop and lock it. If you find yourself making a choice without an ADR, stop and write the ADR. The discipline only pays off if the order holds.

## Output locations

Default layout in the target repo (create directories as needed):

```
<repo-root>/
├── CLAUDE_LOG.md              # append-only session ledger
└── docs/
    ├── specs/
    │   └── <feature>-spec.md  # one per feature, versioned in-file
    └── adr/
        ├── ADR-0001-<slug>.md
        ├── ADR-0002-<slug>.md
        └── ...
```

If the repo already has an established docs convention, follow it instead of imposing this one — match the house style, but keep the three-artifact discipline.

## Reference files

- `references/spec-template.md` — the spec template to fill before coding.
- `references/adr-template.md` — the ADR template (one per decision).
- `references/claude-log-format.md` — CLAUDE_LOG entry format and rules.
- `references/examples.md` — a worked example: a small feature taken from spec → two ADRs → log entries, so you can see what "good" looks like.

Read the relevant template before producing each artifact rather than reconstructing it from memory — the templates carry the exact section ordering reviewers expect.
