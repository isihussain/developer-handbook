# Memory Management: Reference Counting & Cycle Collection (Bacon-Rajan)

Automatic Reference Counting (ARC) reclaims objects deterministically upon pointer discard, using Bacon-Rajan cycle collection algorithms to detect and reclaim cyclic reference graphs.

```mermaid
flowchart LR
    NodeA[Object A: ref_count = 1] -->|Points to| NodeB[Object B: ref_count = 1]
    NodeB -->|Points to: Cyclic Reference!| NodeA
    ExternalRef[External Reference Dropped] -.-> NodeA
    subgraph CycleCollector[Bacon-Rajan Three-Phase Cycle Collection]
        Phase1[Mark Roots: Color candidates Purple] --> Phase2[Trial Deletion: Decrements ref counts, turns Grey/White]
        Phase2 --> Phase3[Reclamation: Reclaims White cyclic nodes, restores Black]
    end
```

---

## 1. Node Color States

* **Black:** In use or already freed; not part of any suspected garbage cycle.
* **Grey:** Visited during trial deletion; reference counts decremented to identify internal cycles.
* **White:** Confirmed cyclic garbage; marked for memory deallocation.
* **Purple:** Possible root of a cycle (flagged when a reference count drops without hitting zero).
* **Trial Deletion:** If decrementing internal pointer counts within a subgraph brings all subgraph reference counts to zero, the entire cluster represents an isolated cyclic garbage island.
