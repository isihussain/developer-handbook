# JIT Compilers: Java HotSpot C2 Escape Analysis Architecture

HotSpot C2 compiler analyzes whether newly allocated objects escape the current method or thread scope. If an object does not escape, heap allocation is eliminated.

```mermaid
flowchart TD
    MethodCall[Java Method: new Point(x, y)] --> EscapeCheck{Does object escape method?}
    EscapeCheck -->|GlobalEscape: Returned / Assigned to Field| HeapAlloc[Allocate on JVM Heap with GC overhead]
    EscapeCheck -->|NoEscape: Used only inside method| ScalarReplacement[Scalar Replacement: Disassembles object into local CPU registers!]
    ScalarReplacement --> LockElim[Lock Elimination: Removes synchronization locks on object]
```

---

## 1. Performance Impact
Eliminates object allocation costs completely, turning object-oriented abstractions into zero-overhead register operations.
