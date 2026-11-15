# Linux Kernel: Scalable Queued Spinlocks (qspinlock)

Queued spinlocks replace legacy ticket spinlocks, using an MCS queue array embedded within a single 32-bit atomic word to prevent cache-line bouncing across multi-socket NUMA systems.

```mermaid
flowchart TD
    subgraph AtomicWord[32-bit Lock Word]
        Owner[Bits 0-7: Locked Byte 0x01]
        Pending[Bit 8: Pending Bit for fast path handoff]
        Tail[Bits 16-31: Tail CPU Core ID + Per-CPU Context Node]
    end
    CPU1[Contending CPU Core 1] -->|Queues in per-CPU MCS Node| MCS1[MCS Node 1: Spins on local cache line]
    CPU2[Contending CPU Core 2] -->|Queues behind CPU 1| MCS2[MCS Node 2: Spins on local cache line]
```

---

## 1. Operating Phases

1. **Fast Path:** If the lock word is `0`, an incoming CPU executes `CAS(0, 1)` and claims ownership in one instruction.
2. **Pending Path:** If locked without contenders, a second CPU sets the `Pending` bit and spins on the main lock byte.
3. **Queued Path:** Subsequent contending cores enqueue into per-CPU MCS nodes, spinning strictly on their local CPU cache lines until the predecessor signals availability.
