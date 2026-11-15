# Network Hardware: NIC Multi-Queue, Receive Side Scaling (RSS) & Flow Director

Modern Network Interface Cards (NICs) eliminate single-core CPU processing bottlenecks by distributing incoming network flows across multiple hardware RX queues using hardware hashing engines.

```mermaid
flowchart TD
    IngressPacket[Incoming Ethernet Packet: 4-Tuple Header] --> HashEngine[Hardware Toeplitz Hash Engine]
    HashEngine --> HashVal[32-bit Hash Value]
    HashVal --> IndirectionTable[Indirection Table (e.g. 128/512 entries)]
    IndirectionTable --> TargetQueue[Target RX Hardware Queue: Queue 3]
    TargetQueue --> MSIX[MSI-X Interrupt fires on designated CPU Core 3]
    TargetQueue --> NAPI[Linux Kernel NAPI Poll on Core 3]
```

---

## 1. Hardware Steering Technologies

1. **Receive Side Scaling (RSS):** Hashes IP and port 4-tuples using a symmetric Toeplitz secret key, indexing an indirection table to select target RX queues.
2. **Intel Flow Director (FDir):** Programmable hardware filter that matches exact packet signatures and steers specific flows directly to designated CPU caches without hash collisions.
3. **Receive Packet Steering (RPS):** Software fallback emulation of RSS executed inside kernel drivers for legacy single-queue NIC hardware.
