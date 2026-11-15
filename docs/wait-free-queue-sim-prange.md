# Wait-Free Queue Architecture & Bounded Progress Guarantees

A wait-free data structure guarantees that every active thread completes its operation in a bounded number of execution steps, regardless of the relative speed or preemption of other concurrent threads.

```mermaid
flowchart TD
    Op[Thread issues Enqueue Operation] --> FastPath[Fast Path: Atomic CAS on Tail]
    FastPath -->|Success| Complete[Operation Complete in O 1 steps]
    FastPath -->|CAS Contention Failure| Announce[Post Operation to Announcement Array]
    Announce --> Help[Helping Scheme: Peer threads execute pending requests]
    Help --> Bound[Guaranteed completion in bounded steps]
```

---

## 1. Concurrency Hierarchy

$$	ext{Blocking} \subset 	ext{Obstruction-Free} \subset 	ext{Lock-Free} \subset 	ext{Wait-Free}$$

* **Lock-Free:** At least one thread makes forward progress in finite steps.
* **Wait-Free:** *Every* thread makes forward progress in bounded steps.
