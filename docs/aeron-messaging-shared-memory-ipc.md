# Low Latency Messaging: Real Logic Aeron IPC Architecture

Aeron provides OSI layer 4 transport over UDP and shared memory (IPC), delivering millions of messages per second with sub-microsecond latencies.

```mermaid
flowchart LR
    Publisher[Publisher Process] -->|Lockless Atomic Append| ShmFile[POSIX Shared Memory: /dev/shm/aeron]
    ShmFile --> Driver[Aeron Media Driver]
    Driver -->|Direct Memory Read| Subscriber[Subscriber Process]
```

---

## 1. Conductor & Media Driver Decoupling
The Media Driver operates out-of-process, reading and writing shared memory ring buffers so that application threads never block on network sockets or kernel traps.
