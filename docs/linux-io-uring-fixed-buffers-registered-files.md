# High-Performance I/O: io_uring Fixed Buffers & Registered Files

Standard I/O requires the kernel to pin and unpin user pages in memory and allocate internal file table structures for every operation. `io_uring` fixed buffers eliminate this overhead.

```mermaid
sequenceDiagram
    participant App as Userspace Application
    participant Kernel as Linux Kernel

    Note over App: Pre-allocates 1GB I/O buffer pool
    App->>Kernel: io_uring_register(IORING_REGISTER_BUFFERS)
    Note over Kernel: Kernel maps and pins buffer pages ONCE at startup!
    App->>Kernel: io_uring_register(IORING_REGISTER_FILES)
    Note over Kernel: Pre-registers socket/disk FDs into kernel array!
    Note over App,Kernel: Subsequent I/O uses index lookup with ZERO page pinning overhead!
```

---

## 1. Performance Impact
Registered buffers increase throughput by 15-20% for high-frequency NVMe storage and 100GbE network packet processing.
