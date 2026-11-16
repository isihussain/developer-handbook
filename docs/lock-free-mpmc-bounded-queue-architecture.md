# Low Latency Data Structures: Dmitry Vyukov MPMC Bounded Queue

Vyukov's lock-free MPMC queue utilizes a circular array where each cell contains a sequence number to coordinate multiple concurrent producers and consumers with zero locks.

```mermaid
flowchart LR
    Producers[Multiple Producer Threads] -->|Atomic CAS on enqueue_pos| Cell[Cell Sequence Check]
    Cell -->|cell.seq == pos| WriteData[Write Payload & cell.seq = pos + 1]
    Consumers[Multiple Consumer Threads] -->|Atomic CAS on dequeue_pos| CellRead[Cell Sequence Check]
    CellRead -->|cell.seq == pos + 1| ReadData[Read Payload & cell.seq = pos + capacity]
```

---

## 1. Invariants
- Enqueue succeeds only if cell sequence equals current turn.
- Dequeue succeeds only if cell sequence equals enqueue turn + 1.
- Operates entirely lock-free with $O(1)$ amortized cost.
