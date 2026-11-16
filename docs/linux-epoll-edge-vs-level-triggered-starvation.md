# Network Programming: Epoll EPOLLET vs Level-Triggered Starvation

Level-triggered epoll reports readability as long as data remains in the buffer. Edge-triggered (`EPOLLET`) notifies only upon state transitions (e.g. from empty to non-empty).

```mermaid
flowchart TD
    DataArrives[Data Arrives at Socket] --> EdgeTriggered{EPOLLET Mode}
    EdgeTriggered --> Notify[Single Notification Dispatched]
    Notify --> ReadLoop[Must Loop read() until -EAGAIN / -EWOULDBLOCK]
    ReadLoop -->|Didn't read to end?| STALL[STALL! Next data arrival will NOT notify!]
```

---

## 1. Worker Starvation Mitigation
In edge-triggered mode, reading in an unbounded loop for one active connection can starve other ready connections. Mitigate by enforcing a maximum read batch per socket before re-arming.
