# Real-Time Linux: RT-Mutex & Priority Inheritance (PI) Protocol

Priority Inheritance eliminates unbounded priority inversion in real-time operating systems by temporarily elevating the priority of a low-priority lock holder to match the highest-priority blocked thread.

```mermaid
sequenceDiagram
    participant High as High-Priority Task (Prio 10)
    participant Med as Medium-Priority Task (Prio 50)
    participant Low as Low-Priority Task (Prio 90)
    Note over Low: Holds Mutex M
    High->>Low: Attempts to acquire Mutex M (Blocks!)
    Note over Low: Priority Inheritance: Low promoted to Prio 10!
    Med->>Low: Med attempts to preempt Low (Blocked: 50 < 10)
    Note over Low: Low finishes critical section and releases Mutex M
    Note over Low: Priority demoted back to Prio 90
    High->>High: High acquires Mutex M and executes immediately
```

---

## 1. Deadlock Detection & Chaining

* **PI Chains:** If Task A blocks on Mutex 1 (held by B), and Task B blocks on Mutex 2 (held by C), priority boosts propagate transitively ($A 	o B 	o C$).
* **Deadlock Walk:** The kernel traverses wait-queues during lock contention; if a cyclic dependency ($C 	o A$) is detected, the call aborts immediately with `-EDEADLK`.
