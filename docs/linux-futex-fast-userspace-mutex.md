# Linux Fast Userspace Mutex (futex) Architecture

Futexes combine atomic user-space memory operations with kernel wait queues to provide zero-syscall locks under low contention.

```mermaid
sequenceDiagram
    autonumber
    Note over ThreadA: Uncontended Fast Path: atomic CAS 0 -> 1 (0 syscalls)
    ThreadB->>ThreadB: Attempts lock: atomic CAS fails (Contended)
    ThreadB->>Kernel: futex(addr, FUTEX_WAIT_PRIVATE, 1, timeout)
    Kernel->>Kernel: Enqueue ThreadB on hash bucket wait-queue & sleep
    Note over ThreadA: Releases lock: atomic exchange 0
    ThreadA->>Kernel: futex(addr, FUTEX_WAKE_PRIVATE, 1)
    Kernel->>ThreadB: Wake up ThreadB
    ThreadB->>ThreadB: Acquires lock in user-space
```

---

## 1. Core Syscall Interface

```c
int futex(uint32_t *uaddr, int futex_op, uint32_t val,
          const struct timespec *timeout, uint32_t *uaddr2, uint32_t val3);
```

- `FUTEX_WAIT`: If `*uaddr == val`, puts the calling thread to sleep.
- `FUTEX_WAKE`: Wakes up to `val` threads sleeping on `uaddr`.
- `FUTEX_PRIVATE_FLAG`: Scopes the wait queue to the current process address space, skipping system-wide inode hash locks.
