# Diagram — Evidence tiers

Not all evidence carries equal weight in an architecture argument. Argue from the higher tiers.

```mermaid
flowchart TB
    T4[Published case study at your scale<br/>strong prior art]
    T3[Benchmark in your env with your data<br/>high weight]
    T2[Anecdote - worked at a previous job<br/>low weight]
    T1[Opinion - I think it will scale<br/>no weight]

    T1 -- promote with evidence --> T2
    T2 -- promote with measurement --> T3
    T3 -- promote with prior art --> T4

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
                       ┌─────────────────────────────┐
                       │ Published case study at     │
                       │ your scale                  │   strong prior art
                       └─────────────────────────────┘
                ┌──────────────────────────────────────────┐
                │ Benchmark in your environment, your data │   high weight
                └──────────────────────────────────────────┘
       ┌─────────────────────────────────────────────────────────┐
       │ Anecdote - worked at a previous job                      │   low weight
       └─────────────────────────────────────────────────────────┘
┌──────────────────────────────────────────────────────────────────────┐
│ Opinion - "I think it will scale"                                    │   no weight
└──────────────────────────────────────────────────────────────────────┘
```

The asymmetry matters: in a Verdict-style discussion, an opinion does not rebut a benchmark. A benchmark does not require an opinion to overturn it — it requires another benchmark or stronger.
