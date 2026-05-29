---
description: Start the spec-first ADR discipline — lock a spec before coding, record an ADR per significant decision, and maintain an append-only CLAUDE_LOG.
---

Engage the **spec-first ADR discipline** for the task described in `$ARGUMENTS` (or the current request if empty).

Follow the `spec-first-adr` skill exactly, in order:

1. **Lock the spec first.** Use `references/spec-template.md`. Write scope, explicit non-goals, interfaces/contracts, and open questions. Do not write implementation code until the user signs off with "locked" / "proceed" / "go". State the lock clearly.
2. **Write an ADR per significant decision.** Use `references/adr-template.md`. Number sequentially (`ADR-0001`, ...), keep accepted ADRs immutable, supersede instead of rewriting, and always list the downsides you accept.
3. **Maintain `CLAUDE_LOG.md`** at the repo root. Append-only, one line per decision/action, newest at the bottom, cross-linked to ADRs.

Default output layout (match the repo's existing docs convention if one exists):

```
<repo-root>/
├── CLAUDE_LOG.md
└── docs/
    ├── specs/<feature>-spec.md
    └── adr/ADR-0001-<slug>.md
```

Read the relevant template before producing each artifact. The ordering — **Spec lock → ADR per decision → CLAUDE_LOG throughout** — is not negotiable.
