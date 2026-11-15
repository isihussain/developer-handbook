# Linux IPC: Anonymous Shared Memory with memfd_create & File Sealing

The `memfd_create()` system call creates anonymous memory-backed file descriptors that support hardware-accelerated zero-copy shared memory (`mmap`) while preventing malicious tampering via File Sealing.

```mermaid
flowchart LR
    ProcA[Process A: Host / Compositor] -->|memfd_create('shm_buf')| FD[Anonymous File Descriptor]
    ProcA -->|fcntl(F_ADD_SEALS, F_SEAL_SHRINK \| F_SEAL_GROW \| F_SEAL_WRITE)| FD
    ProcA -->|SCM_RIGHTS UNIX Socket Passing| ProcB[Process B: Untrusted Client]
    ProcA & ProcB -->|Zero-Copy Direct Access| RAM[RAM Page Cache: Immutable / Sealed]
```

---

## 1. File Seal Flags (`fcntl`)

* **`F_SEAL_SEAL`:** Prevents further seals from being added to the file descriptor.
* **`F_SEAL_SHRINK`:** Prevents truncation or reduction of file size (`ftruncate`), eliminating `SIGBUS` faults caused by unmapping pages under concurrent access.
* **`F_SEAL_GROW`:** Prevents increasing file size beyond its current allocation.
* **`F_SEAL_WRITE`:** Blocks all write operations and writable `mmap` mappings, ensuring complete payload immutability.
