# JDK ZGC: Colored Pointers & Load Barriers

The Z Garbage Collector (ZGC) achieves sub-millisecond maximum pause times regardless of heap size (up to 16TB).

```
[ 64-bit Virtual Address Reference in ZGC ]
+-------------------+---------+-----------------------------------+
| 16 bits (Unused)  | 4 bits  | 44 bits (Object Offset: up to 16TB)|
+-------------------+---------+-----------------------------------+
                       |
                       +-> [ Finalizable | Remapped | Marked1 | Marked0 ]
```

---

## 1. Self-Healing Load Barriers

When an application thread dereferences an object pointer whose color bit indicates relocation:
1. The JIT-compiled **Load Barrier** intercepts the load.
2. Reads the forwarding table to find the object's new address.
3. Updates the local pointer in-place to the new address (**Self-Healing**).
4. Subsequent loads bypass the barrier with zero overhead.
