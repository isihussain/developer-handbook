# Coroutine Internals: Stackful vs Stackless Models

Coroutines are subroutines capable of yielding execution and resuming state without blocking the underlying OS thread.

```mermaid
flowchart LR
    subgraph Stackful [Stackful (Fibers / Go goroutines)]
        S1[Dedicated Stack allocated in heap 2KB-8KB]
        S2[Context switch swaps CPU registers & RSP]
        S3[Can yield from deep nested call stacks]
    end
    subgraph Stackless [Stackless (C++20 / Rust async)]
        SL1[State machine struct in heap/frame]
        SL2[Compiler lowers into switch statement]
        SL3[Can only yield from top-level function]
    end
```

---

## 1. Tradeoffs

| Metric | Stackful (Fibers) | Stackless (C++20 / Rust) |
|---|---|---|
| **Memory Footprint** | 2KB - 8KB per coroutine stack | Tens to hundreds of bytes (exact struct size) |
| **Suspension Depth** | Arbitrary nested calls | Top-level await expressions only |
| **Context Switch Cost** | Register save/restore + stack pointer swap | Function return + jump table update |
