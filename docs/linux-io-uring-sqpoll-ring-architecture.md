# Linux I/O: io_uring Lock-Free Ring Buffer Architecture

Linux `io_uring` enables zero-syscall asynchronous I/O by sharing two ring buffers (Submission Queue and Completion Queue) in mmap'd memory between user space and kernel space.

```mermaid
sequenceDiagram
    participant User as Userspace Application
    participant SQ as Submission Queue (mmap)
    participant Kernel as Kernel Worker (SQPOLL Thread)
    participant CQ as Completion Queue (mmap)

    User->>SQ: Write Submission Queue Entry (SQE)
    User->>SQ: Update SQ Tail Pointer (Lock-free!)
    Note over Kernel: Kernel thread polls SQ Tail without any syscall!
    Kernel->>Kernel: Executes Asynchronous I/O
    Kernel->>CQ: Writes Completion Queue Entry (CQE)
    Kernel->>CQ: Updates CQ Tail Pointer
    User->>CQ: Reads completed results directly from memory!
```

---

## 1. SQPOLL Mode
When configured with `IORING_SETUP_SQPOLL`, a dedicated kernel thread polls the submission queue, allowing complete I/O pipelines without executing a single system call.
