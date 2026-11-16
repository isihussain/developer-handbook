# Secure Multi-Party Computation: Yao Garbled Circuits Protocol

Yao's Garbled Circuits allows two distrusting parties (Alice with input $x$, Bob with input $y$) to compute $f(x, y)$ without revealing their respective private inputs.

```mermaid
sequenceDiagram
    participant Alice as Garbler (Alice: input x)
    participant Bob as Evaluator (Bob: input y)

    Note over Alice: Converts function f into Boolean Logic Gates
    Note over Alice: Encrypts (garbles) truth tables with wire keys
    Alice->>Bob: Sends Garbled Circuit + Alice's Wire Keys for x
    Note over Bob: Obtains wire keys for y via Oblivious Transfer (OT)
    Note over Bob: Evaluates gates deterministically without learning intermediate values!
    Bob-->>Alice: Shared Output f(x, y)
```

---

## 1. Oblivious Transfer Role
1-out-of-2 Oblivious Transfer allows Bob to get keys corresponding to his input bits without Alice learning Bob's bits, and without Bob learning the opposite wire keys.
