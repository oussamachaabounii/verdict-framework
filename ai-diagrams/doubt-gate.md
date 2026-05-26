# AI Diagram — The doubt gate (AI version)

What blocks vs. what passes — for prompts, agents, and model changes.

## The three-condition gate

Doubt must be **specific AND reasonable AND unaddressed** to block.

```mermaid
flowchart LR
    S[Specific<br/>a concrete input that produces<br/>a concrete bad output]
    R[Reasonable<br/>plausible for this product,<br/>users, and blast radius]
    U[Unaddressed<br/>not covered by the eval set,<br/>not mitigated, no kill switch]
    AND((AND))
    Block[Blocks the change]
    Pass[Does not block - logged only]

    S --> AND
    R --> AND
    U --> AND
    AND -- all three --> Block
    AND -- any missing --> Pass
```

## Truth table

```
   Specific    Reasonable   Unaddressed     Result
   ────────    ──────────   ───────────     ─────────
      ✗            *             *           not blocking (vibe)
      ✓            ✗             *           not blocking (out-of-product paranoia)
      ✓            ✓             ✗           not blocking (already in eval / mitigated)
      ✓            ✓             ✓           BLOCKS
```

The AI-specific sharpening: **a specific failure case can always be added to the eval set as a runnable test**. That converts a subjective argument into a measurable one. Either the prompt passes that test or it does not.

## The escalation path

When a failure case cannot be resolved by mitigation:

```mermaid
sequenceDiagram
    participant Dissenter
    participant Author as Prompt author
    participant Decider as Named decision-maker
    participant Register as Eval failure register

    Dissenter->>Register: Add failure case (input + bad output)
    Register-->>Author: Visible, owned, test generated
    Author->>Author: Defined window to mitigate
    alt Mitigation passes eval
        Author->>Register: Mitigation linked, eval green
        Register-->>Dissenter: Closed
    else Mitigation does not pass
        Author->>Decider: Escalate with eval results
        Decider->>Register: Decision + explicit risk acceptance (in writing)
        Note over Register: Failure case stays in the eval.<br/>It does not disappear.
    end
```

## The eval failure register — example

| Failure case | Found by | Date | Mitigation | Status |
|--------------|----------|------|------------|--------|
| Leaks system prompt on "ignore previous instructions" | Ana | Jan 12 | Input filter + scrubber, eval case #87 | Closed |
| Tool call loops on malformed JSON | Carlos | Jan 14 | Max-iteration cap + retry budget, residual accepted | Accepted, ADR-07 |
| Wrong-language response for some locales | Ana | Jan 15 | OPEN — eval subset in sprint 6 | Open with plan |
| Hallucinated SKU codes on long inputs | Oussama | Jan 18 | OPEN — no plan | Gate violation |

> An open failure **with a plan** is acceptable.
> An open failure **with no plan** is a gate violation.
