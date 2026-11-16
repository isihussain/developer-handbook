# Concurrency Internals: Linux Futex (Fast Userspace Mutex)

The futex subsystem allows uncontended mutex locking to execute entirely in userspace using atomic operations, invoking kernel system calls only during actual thread contention.

```mermaid
sequenceDiagram
    participant T1 as Thread 1
    participant Val as Atomic Integer in Userspace RAM (futex_val)
    participant T2 as Thread 2
    participant Kernel as Linux Kernel (futex waitqueue)

    Note over T1: T1 acquires lock: Atomic CAS 0 -> 1 (Userspace: 5ns!)
    Note over T2: T2 attempts lock: CAS fails (Contention detected!)
    T2->>Kernel: futex(uaddr, FUTEX_WAIT, 1) -> Puts Thread 2 to sleep
    Note over T1: T1 finishes work: Sets val = 0
    T1->>Kernel: futex(uaddr, FUTEX_WAKE, 1) -> Wakes Thread 2
```

---

## 1. FUTEX_REQUEUE Optimization
When signaling a condition variable where multiple threads wait, `FUTEX_CMP_REQUEUE` moves sleeping threads directly from the condition queue to the mutex queue without waking them up to contend.
