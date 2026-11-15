# NVMe: Submission & Completion Queue Architecture

Non-Volatile Memory Express (NVMe) eliminates SATA/AHCI single-queue bottlenecks by supporting up to 64,000 paired hardware queues with 64,000 commands each.

```mermaid
sequenceDiagram
    participant Host as Host CPU Driver
    participant SQ as Submission Queue (In Host RAM)
    participant Doorbell as NVMe Controller MMIO Doorbell
    participant CQ as Completion Queue (In Host RAM)
    Host->>SQ: Appends 64-byte command (Read/Write)
    Host->>Doorbell: Writes updated SQ Tail to Doorbell register
    Note over Doorbell: Controller DMAs command from Host RAM
    Note over Doorbell: Controller accesses NAND flash memory
    Doorbell->>CQ: Writes 16-byte completion entry with Phase Tag
    Doorbell->>Host: Issues MSI-X Interrupt (Optional)
    Host->>Doorbell: Writes updated CQ Head to Doorbell register
```

---

## 1. Key Invariants

* **Phase Tag Bit:** Completion entries contain an alternating phase bit, allowing the host driver to detect new completions in circular buffers without memory fences.
* **CPU Core Affinity:** High-performance Linux drivers allocate dedicated Submission/Completion queue pairs per CPU core to prevent cross-core synchronization locks.
