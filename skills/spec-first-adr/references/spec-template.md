# Spec Template

Fill every section. The spec is LOCKED only when scope, non-goals, contracts, and open questions are all resolved or explicitly deferred, and the user has signed off.

---

# Spec: <Feature Name>

- **Version:** 0.1 (Draft) | bump to 1.0 on lock, 1.1+ on revision
- **Status:** Draft | Locked | Superseded
- **Author:** <name>
- **Date:** <YYYY-MM-DD>
- **Related ADRs:** <ADR-XXXX, ...> (fill as they are written)

## 1. Problem

What problem are we solving, and for whom? One short paragraph. If you cannot state the problem without mentioning a solution, you do not yet understand the problem.

## 2. Goals

Bullet list of what success looks like. Each goal should be checkable.

## 3. Non-goals

Explicit list of what this work does NOT cover. This section is as important as the goals — it is the fence that stops scope creep after lock.

## 4. Constraints

Hard limits the design must respect: existing systems, performance budgets, deadlines, tech-stack invariants, compliance, team conventions.

## 5. Interfaces / Contracts

The shape of what the rest of the system sees. Pick what applies:

- **API:** endpoints, methods, request/response schemas, error codes.
- **Data model:** tables/collections, key fields, relationships, migration impact.
- **Events:** event names, payloads, producers/consumers.
- **CLI/Config:** commands, flags, config keys.

Be concrete. Vague contracts produce churn downstream.

## 6. High-level approach

A few sentences on the chosen approach. Deep tradeoffs go in ADRs, not here — this section just sketches the shape so a reader knows what they are about to read code for.

## 7. Open questions

| # | Question | Resolution / Deferral | Owner |
|---|----------|-----------------------|-------|
| 1 |          |                       |       |

Every row must be resolved or explicitly deferred (with an owner) before lock.

## 8. Lock record

- **Locked on:** <YYYY-MM-DD>
- **Locked by:** <who signed off>
- **Revision history:**
  - 1.0 — initial lock
  - 1.1 — <what changed and why>
