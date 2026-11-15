# Concurrency: Flat Combining Synchronization Architecture

Flat Combining reduces cache-line contention on shared data structures by designating one contending thread as the Combiner, which batches and executes operations on behalf of all peer threads.

```mermaid
flowchart TD
    subgraph Threads[Contending Threads]
        T1[Thread 1] -->|Publishes Op| Slot1[Publication Slot 1: Op Append]
        T2[Thread 2] -->|Publishes Op| Slot2[Publication Slot 2: Op Pop]
        T3[Thread 3] -->|Wins Global Lock| Combiner[Combiner Thread]
    end
    Combiner -->|Traverses Publication Array| BatchExec[Executes all operations locally in cache]
    BatchExec --> UpdateResults[Writes results directly to Publication Slots]
    UpdateResults --> ReleaseLock[Combiner releases lock: Threads read answers]
```

---

## 1. Operating Mechanics

1. **Publication Array:** Contending threads write their requested operations into thread-local slots within a shared publication list.
2. **Combiner Election:** The first thread to acquire the global spinlock becomes the Combiner; all other threads sleep or spin on their local publication slot response fields.
3. **Single Cache Domain:** The Combiner iterates through the array, performing all queued mutations sequentially inside its private L1/L2 cache without cross-core cache invalidation traffic.
