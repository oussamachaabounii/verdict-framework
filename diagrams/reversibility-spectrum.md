# Diagram — Reversibility spectrum

The death-penalty axis. Doubt threshold scales with how hard a decision is to undo.

```mermaid
flowchart LR
    A["Fully reversible<br/>(feature flags,<br/>internal impl)"]
    B["Partially reversible<br/>(service boundaries,<br/>API contracts,<br/>framework choices)"]
    C["Effectively irreversible<br/>(public APIs,<br/>data models at scale,<br/>vendor lock-in)"]

    A --> B --> C

    classDef green fill:#064e3b,stroke:#6ee7b7,color:#d1fae5;
    classDef amber fill:#78350f,stroke:#fcd34d,color:#fef3c7;
    classDef red fill:#7f1d1d,stroke:#fecaca,color:#fee2e2;
    class A green;
    class B amber;
    class C red;
```

ASCII version with thresholds:

```
   ◄──── easier to undo                       harder to undo ────►
   ┌──────────────┬────────────────────┬──────────────────────┐
   │   FULLY      │     PARTIALLY      │     EFFECTIVELY      │
   │  reversible  │     reversible     │     irreversible     │
   ├──────────────┼────────────────────┼──────────────────────┤
   │ feature flag │ service boundary   │ public API           │
   │ internal impl│ API contract       │ data model at scale  │
   │ helper code  │ framework choice   │ vendor lock-in       │
   ├──────────────┼────────────────────┼──────────────────────┤
   │ low doubt    │ medium doubt       │ MAXIMUM doubt        │
   │ threshold    │ threshold          │ threshold            │
   │              │                    │                      │
   │ standard PR  │ RFC + sign-off     │ RFC + spike +        │
   │              │ + checkpoint       │ external review +    │
   │              │                    │ leadership sign-off  │
   └──────────────┴────────────────────┴──────────────────────┘
```

The 12-month question: **what would it cost to undo this in 12 months?**

| Cost to undo | Threshold | Process |
|--------------|-----------|---------|
| Hours to days | Low | Standard PR review |
| Weeks | Medium | RFC + team sign-off |
| Months | High | RFC + spike + stakeholder sign-off |
| Unknown / existential | Maximum | RFC + spike + external review + leadership sign-off |

---

## Novelty × Reversibility

Novelty is fine in reversible places. Novelty in irreversible places is the danger zone.

```mermaid
quadrantChart
    title Where novelty is safe vs dangerous
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
