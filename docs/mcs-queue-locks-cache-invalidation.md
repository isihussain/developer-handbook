# MCS Queue Locks: Local Spinning & Cache Coherence Optimization

Mellor-Crummey and Scott (MCS) locks eliminate the $O(P)$ bus invalidation storm of ticket spinlocks by having each waiting CPU core spin exclusively on its own local thread-allocated node.

```mermaid
flowchart LR
    Tail[Global Tail Pointer] --> Q3[Node 3: Core 3 spinning on Core3.locked]
    Q3 --> Q2[Node 2: Core 2 spinning on Core2.locked]
    Q2 --> Q1[Node 1: Core 1 in Critical Section]
    Q1 -->|Unlock: writes Core2.locked = 0| Q2
```

---

## 1. Invalidation Comparison

* **Standard Spinlock:** $P$ cores spin on a single shared memory location; unlocking triggers $O(P)$ bus invalidate transactions across the L3/UPI interconnect.
* **MCS Queue Lock:** Unlocking modifies only the immediate successor node, resulting in strictly $O(1)$ cache coherence traffic.
