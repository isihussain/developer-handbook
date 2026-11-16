# Low-Level Concurrency: SMP Memory Barriers (smp_mb, smp_rmb, smp_wmb)

Modern multi-core out-of-order CPUs (x86, ARM, RISC-V) reorder memory loads and stores for performance unless explicit memory barrier instructions are inserted.

```mermaid
sequenceDiagram
    participant Core1 as CPU Core 1 (Producer)
    participant RAM as Shared Memory Cache
    participant Core2 as CPU Core 2 (Consumer)

    Note over Core1: data = 42;
    Note over Core1: smp_wmb(); (Store Barrier: Guarantees data write visible before flag)
    Note over Core1: flag = 1;
    Core1->>RAM: Write data and flag
    Note over Core2: while (flag == 0);
    Note over Core2: smp_rmb(); (Load Barrier: Guarantees data read after flag)
    Note over Core2: assert(data == 42); (ALWAYS PASSES!)
```

---

## 1. Barrier Primitives
- `smp_rmb()`: Prevents loads from floating before earlier loads.
- `smp_wmb()`: Prevents stores from floating after later stores.
- `smp_mb()`: Full two-way barrier enforcing total memory ordering.
