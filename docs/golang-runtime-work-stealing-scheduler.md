# Go Runtime M:N Work-Stealing Scheduler Architecture

The Go runtime orchestrates $N$ user-level goroutines ($G$) onto $M$ operating system threads through $P$ logical processors, balancing workloads using lock-free local runqueues and random victim work-stealing.

```mermaid
flowchart TD
    subgraph P0[Logical Processor P0]
        LRQ0[Local Runqueue: 256 Gs max]
        M0[OS Thread M0]
        Cur0[Running Goroutine G]
    end
    subgraph P1[Logical Processor P1]
        LRQ1[Local Runqueue: Empty]
        M1[OS Thread M1]
    end
    Cur0 --> M0
    M1 -.->|Steals half of LRQ0| LRQ0
    GRQ[(Global Run Queue)] -.->|Every 61 ticks check GRQ| P0
```

---

## 1. Work-Stealing Algorithm

1. Check local runqueue `P.runq` (lock-free ring buffer).
2. Every 61 ticks, poll global runqueue `sched.runq` to prevent starvation.
3. Check network poller (`netpoll`) for unblocked I/O events.
4. Pick random processor $P_{	ext{victim}}$ and steal $\lfloor N / 2 floor$ goroutines from its local queue.
