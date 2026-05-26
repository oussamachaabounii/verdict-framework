# AI Diagram — Blast radius

The death-penalty axis for AI. Doubt threshold scales with the autonomy of the AI **and** the side effects of its actions.

```mermaid
flowchart LR
    A["Chat reply<br/>(human reads + re-rolls)"]
    B["Embedded system prompt<br/>(silent shift across all users)"]
    C["Agent with tools + side effects<br/>(email, code exec, payments, deletes)"]

    A --> B --> C

    classDef green fill:#064e3b,stroke:#6ee7b7,color:#d1fae5;
    classDef amber fill:#78350f,stroke:#fcd34d,color:#fef3c7;
    classDef red fill:#7f1d1d,stroke:#fecaca,color:#fee2e2;
    class A green;
    class B amber;
    class C red;
```

ASCII view with thresholds:

```
   ◄──── low blast radius                    high blast radius ────►
   ┌───────────────┬─────────────────────┬───────────────────────┐
   │  CHAT REPLY   │  SYSTEM PROMPT      │   AGENT + TOOLS       │
   │  (one-shot)   │  IN PRODUCT         │   + SIDE EFFECTS      │
   ├───────────────┼─────────────────────┼───────────────────────┤
   │ human reads   │ silent behavior     │ sends email, runs     │
   │ + re-rolls    │ shift across all    │ code, moves money,    │
   │               │ users               │ deletes data          │
   ├───────────────┼─────────────────────┼───────────────────────┤
   │ low doubt     │ medium doubt        │ MAXIMUM doubt         │
   │ threshold     │ threshold           │ threshold             │
   │               │                     │                       │
   │ eval recommend│ eval + variance     │ red-team gate +       │
   │               │ + revert plan       │ external review +     │
   │               │                     │ kill switch + audit   │
   └───────────────┴─────────────────────┴───────────────────────┘
```

The worst-plausible-action question: **if this AI takes the worst plausible action one time, what does it cost to undo?**

| Cost to undo | Threshold | Process |
|--------------|-----------|---------|
| User re-rolls | Low | PR review |
| Recoverable wrong answer | Medium | Eval + variance bands |
| Message sent / code deployed | High | Eval + adversarial + revert + HITL |
| Payment / delete / real-world action | Maximum | Red-team gate + external review + kill switch + leadership |

---

## Novelty × blast radius

Novel models are fine in low-blast contexts. Novel models in high-blast contexts are the danger zone.

```mermaid
quadrantChart
    title Model novelty vs action blast radius
    x-axis "Reversible action" --> "Irreversible action"
    y-axis "Proven model" --> "Novel model"
    quadrant-1 "Danger zone"
    quadrant-2 "Sandbox - safe to explore"
    quadrant-3 "Low-risk default"
    quadrant-4 "Boring is the right answer"
    "Brand-new model in autonomous payment agent": [0.9, 0.9]
    "New model in a chat playground": [0.15, 0.85]
    "Mature model with broad tool access": [0.85, 0.2]
    "Mature model, chat-only assistant": [0.15, 0.15]
```
