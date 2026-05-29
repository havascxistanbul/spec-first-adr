# ADR Template

One file per decision: `ADR-NNNN-<slug>.md`. Number sequentially and never reuse a number. Once `Accepted`, never rewrite the body — supersede with a new ADR instead.

---

# ADR-NNNN: <Short title of the decision>

- **Status:** Proposed | Accepted | Superseded by ADR-XXXX | Deprecated
- **Date:** <YYYY-MM-DD>
- **Deciders:** <names>
- **Related:** <spec link, related ADRs>

## Context

What is the situation that forces a decision? What constraints and forces are at play? State the problem neutrally — do not pre-load the answer here. A reader who disagrees with the final choice should still agree with this section.

## Decision

The choice, stated in one or two clear sentences. Active voice: "We will use X."

## Alternatives considered

List the real options that were on the table, each with a one-line reason it was or wasn't chosen. An ADR with no alternatives is a decision that wasn't actually examined.

- **Option A — <name>:** <why / why not>
- **Option B — <name>:** <why / why not>
- **Option C — do nothing:** <why / why not>

## Consequences

What becomes easier, and — critically — what becomes harder. List the downsides you are knowingly accepting. This is the section reviewers read first. If it only contains benefits, the decision is not honest yet.

- Positive: ...
- Negative / accepted cost: ...
- Follow-up needed: ...
