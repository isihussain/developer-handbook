# Storage Engines: B+ Tree Latch Crabbing Protocol

Latch crabbing (coupling) allows concurrent threads to traverse and modify a B+ Tree without holding root-to-leaf locks simultaneously.

```mermaid
flowchart TD
    Root[Root Node] -->|Acquire Read Latch| Child[Child Internal Node]
    Note over Root,Child: If Child is SAFE, release Root Latch!
    Child -->|Acquire Read Latch| Leaf[Leaf Node]
    Note over Child,Leaf: Release Child Latch!
    Leaf --> ReadOperation[Execute Row Read on Leaf]
```

---

## 1. Safety Criteria for Writes
- A node is **safe on insert** if its current key count $< 2d$ (will not split).
- A node is **safe on delete** if its current key count $> d$ (will not merge or borrow).
