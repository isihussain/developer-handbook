# Streaming Systems: Apache Kafka Zero-Copy sendfile() Architecture

Kafka avoids JVM memory heap overhead for message storage, relying entirely on the Linux kernel Page Cache and the `sendfile()` zero-copy system call.

```mermaid
sequenceDiagram
    participant Consumer
    participant Kernel as Linux Kernel (Page Cache)
    participant NIC as Network Interface Card
    participant Disk as Physical Storage

    Note over Kernel: sendfile(out_fd, in_fd, offset, count)
    Disk->>Kernel: DMA Copy: Disk to Page Cache
    Kernel->>NIC: DMA Copy: Page Cache directly to NIC Buffer!
    NIC-->>Consumer: Network Packet
    Note over Kernel,NIC: Zero CPU copies! Data never enters JVM userspace!
```

---

## 1. Memory Invariance
Kafka runs comfortably with small JVM heaps (e.g. 6-8 GB) while utilizing 64-128 GB of host RAM as Linux Page Cache.
