# CRDTs: State-Based (CvRDT) vs Operation-Based (CmRDT)

Conflict-Free Replicated Data Types resolve concurrent updates across peer-to-peer networks without distributed locks, converging deterministically through algebraic properties.

```mermaid
flowchart LR
    subgraph CvRDT[State-Based: CvRDT]
        S1[Replica A State] -->|Transmit full payload| Join[Lattice Join: LUB]
        S2[Replica B State] -->|Transmit full payload| Join
        Join --> S_Final[Converged State: Commutative, Associative, Idempotent]
    end
    subgraph CmRDT[Operation-Based: CmRDT]
        Op1[Local Update] --> Prepare[Prepare Phase: Side-effect free]
        Prepare -->|Causal broadcast: Exactly-once/At-least-once| Effect[Effect Phase: Mutates Replica]
    end
```

---

## 1. Mathematical Requirements

* **CvRDT Join-Semilattice:** States form a partial order with a Least Upper Bound (LUB) join operator ($\sqcup$) satisfying:
  * **Associativity:** $(a \sqcup b) \sqcup c = a \sqcup (b \sqcup c)$
  * **Commutativity:** $a \sqcup b = b \sqcup a$
  * **Idempotence:** $a \sqcup a = a$
* **CmRDT Invariants:** Operations must commute with all concurrent operations. Messaging layers must enforce causal ordering.
