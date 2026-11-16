# Linux Zero-Copy: splice() and vmsplice() Kernel Pipeline

The `splice()` system call moves data between two file descriptors (where at least one is a pipe) without copying bytes into userspace memory.

```mermaid
sequenceDiagram
    participant Source as Network Socket (FD in)
    participant Pipe as Kernel Pipe Buffer (struct pipe_inode_info)
    participant Target as File on Disk (FD out)

    Note over Source,Target: splice(fd_in, NULL, pipe_fd, NULL, len, SPLICE_F_MOVE)
    Source->>Pipe: Direct Page Reference Transfer (No Copy!)
    Pipe->>Target: Page Pointers transferred to target page cache!
    Note over Pipe,Target: Zero memory buffers allocated in user application!
```

---

## 1. Performance Impact
Splice reduces CPU usage by 60% compared to standard `read()` + `write()` loops for high-bandwidth proxy daemons (e.g. HAProxy).
