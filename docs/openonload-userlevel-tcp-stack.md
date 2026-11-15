# OpenOnload User-Level TCP/IP Stack Architecture

OpenOnload intercepts standard POSIX socket API calls via dynamic library linking (`LD_PRELOAD`), routing network traffic entirely in user-space without kernel system calls or context switches.

```mermaid
flowchart TD
    App[Socket Application: socket, epoll, send, recv] --> Intercept[libonload.so: Intercept POSIX Calls]
    Intercept --> FastPath{Can bypass kernel?}
    FastPath -->|Yes: Accelerated Interface| UserTCP[User-Level TCP/IP Stack in User Memory]
    UserTCP --> DirectDMA[Direct Hardware NIC DMA]
    FastPath -->|No: Local IPC or non-Onload NIC| Kernel[Standard Linux Kernel Syscall Path]
```

---

## 1. Intercept Mechanisms

* **Zero Application Code Changes:** Operates transparently on existing binary applications using `onload ./trading_engine`.
* **Shared Memory Sockets:** Sockets connecting processes on the same host transfer packets through lock-free shared memory rings, bypassing physical loopback hardware.
