# Principle 4 — Irreversibility as the Death Penalty

> **Core idea:** The degree of certainty required before deciding should scale with how hard the decision is to undo.

In legal systems with capital punishment, the standard is theoretically higher because the consequence is irreversible. The same logic applies to architecture: the harder a decision is to reverse, the higher the doubt threshold before committing.

---

## The reversibility spectrum

```mermaid
flowchart LR
    A[Fully reversible<br/>feature flags<br/>internal impl] --> B[Partially reversible<br/>service boundaries<br/>API contracts<br/>framework choices] --> C[Effectively irreversible<br/>public APIs<br/>data models at scale<br/>vendor lock-in]

    A -. low doubt threshold .-> Note1[Note concerns, proceed]
    B -. medium threshold .-> Note2[Spike required, checkpoint defined]
    C -. maximum threshold .-> Note3[Death penalty decision]

    classDef green fill:#064e3b,stroke:#6ee7b7,color:#d1fae5;
    classDef amber fill:#78350f,stroke:#fcd34d,color:#fef3c7;
    classDef red fill:#7f1d1d,stroke:#fecaca,color:#fee2e2;
    class A green;
    class B amber;
    class C red;
```

ASCII view:

```
   reversibility
   ────────────────────────────────────────────────►
   ┌──────────────┬────────────────────┬──────────────┐
   │   FULLY      │     PARTIALLY      │  EFFECTIVELY │
   │  reversible  │     reversible     │ irreversible │
   ├──────────────┼────────────────────┼──────────────┤
   │ feature flag │ service boundary   │ public API   │
   │ internal impl│ API contract       │ data model   │
   │              │ framework choice   │ vendor lock  │
   └──────────────┴────────────────────┴──────────────┘
     low doubt        medium doubt        MAX doubt
     threshold         threshold          threshold
```

---

## The reversibility gate

Before any significant decision, ask: **what would it cost to undo this in 12 months?**

| Cost to undo (12 months) | Doubt threshold | Process required |
|--------------------------|-----------------|------------------|
| Hours to days | Low | Standard PR review |
| Weeks | Medium | RFC + team sign-off |
| Months | High | RFC + spike + stakeholder sign-off |
| Unknown / potentially existential | Maximum | RFC + spike + external review + leadership sign-off |

```mermaid
flowchart TD
    Q{Cost to undo in 12 months?}
    Q -- hours/days --> PR[Standard PR review]
    Q -- weeks --> RFC1[RFC + team sign-off]
    Q -- months --> RFC2[RFC + spike + stakeholder sign-off]
    Q -- unknown --> RFC3[RFC + spike + external review + leadership]
```

---

## Designing for reversibility

Reversibility is an architecture quality worth investing in:

- Wrap external dependencies behind interfaces
- Keep business logic out of the database
- Version APIs from day one, even internally
- Use the **strangler fig** pattern for migrations — never hard cutovers
- Prefer proven technology for irreversible choices

> **Novelty and irreversibility are a dangerous combination.**

```mermaid
quadrantChart
    title Novelty vs Reversibility
    x-axis "Reversible" --> "Irreversible"
    y-axis "Proven" --> "Novel"
    quadrant-1 "Danger zone"
    quadrant-2 "Sandbox - safe to explore"
    quadrant-3 "Low-risk default"
    quadrant-4 "Boring is the right answer"
    "Public API on cutting-edge framework": [0.85, 0.85]
    "Feature flag using a new library": [0.15, 0.8]
    "Vendor lock with proven tech": [0.85, 0.2]
    "Internal helper, mature lib": [0.15, 0.15]
```

---

## Tension to watch

Taken too far, this principle produces paralysis. Everything starts to feel irreversible. The classification must be done collaboratively and honestly — and not used strategically to avoid committing.

The mechanism that actually enforces all four prior principles is [Principle 5 — the gate](./05-doubt-gate.md).
