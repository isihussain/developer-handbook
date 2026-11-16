# In-Memory Cache: Redis BGREWRITEAOF and Copy-On-Write Memory Spikes

Redis periodically compacts its Append-Only File (AOF) by calling `fork()` to generate a background child process that rebuilds current state.

```mermaid
flowchart TD
    BGREWRITEAOF[BGREWRITEAOF Triggered] --> Fork[fork() Child Process: Shares memory with Parent via COW]
    ParentMutations[High-Traffic Incoming Writes to Parent] --> PageFault[Copy-On-Write Page Faults: Allocates fresh 4KB pages in RAM!]
    PageFault --> MemorySpike[Host RAM Usage Doubles during AOF rewrite!]
    MemorySpike --> OOM[Potential Linux OOM Killer Termination!]
```

---

## 1. Mitigations
- Overcommit memory setting: `vm.overcommit_memory = 1`.
- Disable Linux Transparent Hugepages (THP) so COW faults duplicate only 4KB pages instead of entire 2MB blocks.
