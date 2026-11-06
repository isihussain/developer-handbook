# LMAX Disruptor High-Throughput Ring Buffer

The Disruptor is an ultra-low-latency inter-thread messaging architecture designed by LMAX.

```mermaid
graph TD
    Producer[Producer Thread] -->|Claim sequence N| Ring[Pre-Allocated Ring Buffer Array]
    Ring --> Barrier[Sequence Barrier]
    Barrier --> Consumer[Consumer Thread (Batch Read)]
```

---

## 1. Design Principles

1. **Pre-Allocation**: Ring buffer slots are allocated at startup, eliminating runtime GC/memory allocation.
2. **Power-of-Two Indexing**: Fast sequence lookup using bitwise AND: `index = seq & (capacity - 1)`.
3. **Cache Line Padding**: Sequences are padded to 64 bytes to eliminate false sharing.
4. **Batch Processing**: Consumers read available sequences in bulk, amortizing synchronization costs.
