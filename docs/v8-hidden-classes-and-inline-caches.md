# V8 Internals: Hidden Classes (Maps) and Inline Caches (IC)

V8 achieves near-C++ object property access speeds in dynamic JavaScript by dynamically synthesizing hidden classes (Maps) and caching property offsets.

```mermaid
flowchart LR
    ObjA["const p = {x: 10};"] --> Map0[Map 0: Offset 0 = x]
    ObjA --> AddY["p.y = 20;"]
    AddY --> Map1[Map 1: Offset 0 = x, Offset 1 = y]
    subgraph InlineCache
        Callsite["p.x lookup"] --> Monomorphic[Monomorphic IC: Fast direct memory offset read: 0ns!]
    end
```

---

## 1. Megamorphic Degradation
When a single call site receives objects with more than 4 distinct hidden class shapes, the inline cache degrades to **Megamorphic**, falling back to hash table lookups.
