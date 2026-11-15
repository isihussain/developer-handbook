# Apache Kafka: Zero-Copy Sendfile & KRaft Quorum

Kafka delivers multi-gigabit throughput by routing network streams directly through the OS kernel and replacing ZooKeeper with an integrated Raft-based metadata quorum (KRaft).

```mermaid
flowchart LR
    subgraph Classic[Standard Transfer: 4 Context Switches / 3 Copies]
        Disk1[Disk File] --> KernelBuf1[OS Page Cache]
        KernelBuf1 --> AppBuf[JVM Application Heap]
        AppBuf --> SocketBuf[Kernel Socket Buffer]
        SocketBuf --> NIC1[Network Card]
    end
    subgraph ZeroCopy[sendfile System Call: 2 Context Switches / 0 CPU Copies]
        Disk2[Disk File] --> KernelBuf2[OS Page Cache]
        KernelBuf2 -->|DMA Transfer directly| NIC2[Network Card]
    end
```

---

## 1. Engine Highlights

1. **Linux `sendfile()`:** Data moves directly from kernel page cache buffers to the network interface card via DMA, bypassing JVM heap allocations.
2. **KRaft Metadata Quorum:** Uses an event-driven Raft protocol inside Kafka brokers to eliminate ZooKeeper latency spikes and speed up cluster recovery.
