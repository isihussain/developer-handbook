# Linux IPC: POSIX Message Queues (mq_overview) vs UNIX Domain Sockets

UNIX Domain Sockets provide byte-stream or datagram communication, whereas POSIX Message Queues deliver prioritized message boundaries with kernel notification mechanisms.

```mermaid
flowchart TD
    Producer[Producer Process] --> mq_send[mq_send(): Assigns Priority 0-31]
    mq_send --> KernelQueue[Kernel POSIX Queue: /dev/mqueue]
    KernelQueue --> PrioritySort[Higher priority messages delivered first!]
    KernelQueue --> Consumer[Consumer Process: mq_receive() or mq_notify() signal]
```

---

## 1. Comparison Matrix
- **UNIX Domain Sockets:** Higher streaming throughput; supports file descriptor passing (`SCM_RIGHTS`).
- **POSIX Message Queues:** True priority-based delivery; guaranteed message boundaries without user framing.
