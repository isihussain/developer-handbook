# Concurrency Internals: Epoll Thundering Herd and EPOLLEXCLUSIVE

When multiple worker processes monitor the same listening socket with epoll, an incoming connection traditionally wakes all workers, causing massive CPU contention.

```mermaid
flowchart TD
    IncomingSYN[Incoming TCP Connection] --> EventPoll{Kernel epoll waitqueue}
    EventPoll -->|Without EPOLLEXCLUSIVE| ThunderingHerd[Wakes ALL 32 Nginx Workers: 1 succeeds, 31 return EAGAIN!]
    EventPoll -->|With EPOLLEXCLUSIVE| SingleWake[Kernel wakes exactly ONE worker thread: O(1) CPU efficiency!]
```

---

## 1. Flag Semantics
`EPOLLEXCLUSIVE` (introduced in Linux 4.5) ensures that only one of the epoll file descriptors waiting on the shared socket is woken up for an event.
