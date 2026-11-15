# ScyllaDB: Thread-per-Core Architecture & Seastar Engine

ScyllaDB replaces JVM garbage collection and thread synchronization with the Seastar C++ framework, employing a shared-nothing, thread-per-core asynchronous model.

```mermaid
flowchart TD
    subgraph Core0[CPU Core 0: Pinned Thread]
        Mem0[Dedicated RAM Partition: 16 GB]
        Reactor0[Async Event Loop: io_uring / epoll]
    end
    subgraph Core1[CPU Core 1: Pinned Thread]
        Mem1[Dedicated RAM Partition: 16 GB]
        Reactor1[Async Event Loop: io_uring / epoll]
    end
    Core0 -.->|Non-blocking SPSC Ring Buffer| Core1
```

---

## 1. Design Principles

1. **Core Isolation:** Each OS thread pins to a dedicated CPU core with its own memory partition, avoiding cross-core cache invalidation.
2. **Lockless Communication:** Inter-thread communication uses Single-Producer Single-Consumer (SPSC) ring queues instead of mutexes.
3. **Autonomous I/O Scheduler:** An integrated I/O scheduler prioritizes user reads over background SSTable compaction to guarantee sub-millisecond p99 latencies.
