# Linux epoll vs. io_uring for Trading Gateways

While pinned matching cores avoid OS system calls, management and client gateways utilize Linux io_uring to submit batch network requests without context switches.

```mermaid
flowchart TD
    subgraph epoll_model[epoll System Call Model]
        App1[App] -->|syscall: epoll_wait| Kernel1[Kernel]
        App1 -->|syscall: read / write per socket| Kernel1
        note1[Multiple context switches per event loop]
    end

    subgraph io_uring_model[io_uring Lock-Free Model]
        App2[App] -->|Write SQEs| SQ[Shared Submission Queue Ring]
        Kernel2[Kernel Worker Thread / IORING_SETUP_SQPOLL] -->|Pulls SQEs with 0 syscalls| SQ
        Kernel2 -->|Writes CQEs| CQ[Shared Completion Queue Ring]
        App2 -->|Reads CQEs with 0 syscalls| CQ
    end
```

---

## 1. Feature Comparison

| Metric | Linux epoll | Linux io_uring (SQPOLL mode) |
|---|---|---|
| I/O Submission Method | Synchronous system calls (`write`, `sendto`) | Memory-mapped lock-free ring buffer |
| Context Switch Cost | 200–400 ns per `epoll_wait` + I/O call | **0 ns** (Worker kernel thread polls ring) |
| Buffer Registration | No (Buffer mapped/unmapped dynamically) | Fixed buffers (`IORING_REGISTER_BUFFERS`) |
| Scalability | Degrades past 10,000 active connections | Scales linearly via completion rings |
