# Diagram — The doubt gate

What blocks vs. what passes.

## The three-condition gate

Doubt must be **specific AND reasonable AND unaddressed** to block.

```mermaid
flowchart LR
    S[Specific<br/>named, concrete failure mode]
    R[Reasonable<br/>a competent engineer would<br/>consider it a real risk]
    U[Unaddressed<br/>not answered in RFC,<br/>spike, or discussion]
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
      ✓            ✗             *           not blocking (paranoia)
      ✓            ✓             ✗           not blocking (already addressed)
      ✓            ✓             ✓           BLOCKS
```

## The escalation path

When doubt cannot be resolved by discussion:

```mermaid
sequenceDiagram
    participant Dissenter
    participant Proposer
    participant Decider as Named decision-maker
    participant Register as Doubt register

    Dissenter->>Register: Articulate doubt in writing
    Register-->>Proposer: Visible, owned
    Proposer->>Proposer: Defined window to address
    alt Resolved in window
        Proposer->>Register: Resolution + owner
        Register-->>Dissenter: Closed
    else Unresolved
        Proposer->>Decider: Escalate
        Decider->>Register: Decision + explicit risk acceptance (in writing)
        Note over Register: Doubt recorded as unresolved.<br/>It does not disappear.
    end
```

## The doubt register — example

| Doubt raised | Raised by | Date | Resolution | Resolved by |
|--------------|-----------|------|------------|-------------|
| No circuit breaker on downstream calls | Ana | Jan 12 | Added Polly retry + circuit breaker, PR #412 | Oussama |
| Redis single point of failure | Carlos | Jan 14 | Accepted risk — revisit at 50k users, ADR-07 | Team |
| Schema migration plan for v2 unclear | Ana | Jan 15 | OPEN — spike scheduled for sprint 6 | Oussama |

> An open doubt **with a plan** is acceptable.
> An open doubt **with no plan** is a gate violation.
