# Microarchitecture: False Sharing and Cache Line Contention

False sharing occurs when independent variables accessed by different CPU cores reside on the same 64-byte L1 cache line, causing constant cache invalidations.

```mermaid
flowchart TD
    subgraph Problem[False Sharing: 1 Cache Line (64 bytes)]
        VarA[Thread 1 writes varA: 8 bytes]
        VarB[Thread 2 writes varB: 8 bytes]
    end
    VarA -->|Invalidates Entire 64-byte Line!| CacheMiss[Thread 2 suffers L1 Cache Miss on varB!]
    subgraph Solution[Cacheline Padding: #[repr(align(64))]]
        PadA[varA: 64 bytes isolated]
        PadB[varB: 64 bytes isolated]
    end
    Solution --> ZeroMiss[Zero cross-core cache invalidation!]
```

---

## 1. Rust / C++ Implementation
```rust
#[repr(align(64))]
struct PaddedCounter {
    value: AtomicU64,
}
```
