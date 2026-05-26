# AI Diagram — Evidence tiers

Not all evidence carries equal weight in an AI argument. Argue from the higher tiers.

```mermaid
flowchart TB
    T4[Published benchmark + your adversarial eval<br/>pass-rate with confidence interval]
    T3[Your eval set on a representative distribution<br/>scored, with variance bands]
    T2[I tried 5 examples in the playground<br/>low weight]
    T1[Feels right - the model is smart<br/>no weight]

    T1 -- promote with examples --> T2
    T2 -- promote with a scored eval --> T3
    T3 -- promote with adversarial subset + CIs --> T4

    classDef weak fill:#374151,stroke:#9ca3af,color:#f3f4f6;
    classDef mid  fill:#78350f,stroke:#fcd34d,color:#fef3c7;
    classDef strong fill:#064e3b,stroke:#6ee7b7,color:#d1fae5;
    class T1 weak;
    class T2 weak;
    class T3 mid;
    class T4 strong;
```

ASCII pyramid:

```
                       ┌─────────────────────────────────────┐
                       │ Benchmark + adversarial eval +      │
                       │ pass rate with CI                   │   strong prior art
                       └─────────────────────────────────────┘
                ┌──────────────────────────────────────────────┐
                │ Eval set on representative distribution,     │   high weight
                │ scored, variance bands                       │
                └──────────────────────────────────────────────┘
       ┌─────────────────────────────────────────────────────────┐
       │ "I tried 5 examples in the playground"                  │   low weight
       └─────────────────────────────────────────────────────────┘
┌──────────────────────────────────────────────────────────────────────┐
│ "Feels right" / "the model is smart"                                 │   no weight
└──────────────────────────────────────────────────────────────────────┘
```

Two AI-specific notes:

- **A single passing run is not evidence.** Generation is non-deterministic — you need a pass *rate*, with enough samples to bound variance.
- **An eval without adversarial cases is incomplete.** Happy-path-only evals cannot rule out injection, jailbreaks, or tool misuse.

In a reasonable-doubt discussion, an anecdote does not rebut a scored eval, and a scored eval without adversarial cases does not rebut a red-team finding.
