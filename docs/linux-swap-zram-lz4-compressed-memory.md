# Linux Memory: ZRAM LZ4 Compressed In-Memory Swap Architecture

ZRAM creates a virtual block device in RAM where swapped pages are dynamically compressed with LZ4, achieving a 2x-3x memory capacity multiplier.

```mermaid
flowchart LR
    AppMem[Application Anonymous Memory Pressure] --> PageOut[Kernel Page Reclaim: swap_out()]
    PageOut --> ZRAM_Driver[ZRAM Block Device: /dev/zram0]
    ZRAM_Driver --> LZ4[Fast LZ4 Compression: 3GB RAM compressed into 1GB pool!]
    LZ4 --> Zsmalloc[zsmalloc slab allocator: Low memory fragmentation]
```

---

## 1. Performance vs Disk Swap
- Disk / NVMe swap latency: ~50-100 microseconds.
- ZRAM memory compression latency: ~1-2 microseconds (50x faster!).
