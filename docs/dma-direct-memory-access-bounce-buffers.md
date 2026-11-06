# Direct Memory Access (DMA) & Bounce Buffers

DMA permits peripheral hardware (NICs, NVMe controllers) to read and write physical host RAM directly without CPU intervention.

```mermaid
flowchart LR
    Dev[Peripheral Device] <-->|Bus Master DMA| RAM[Physical Memory Pages]
    Dev -.->|Device lacks 64-bit addressing| Bounce[Low 32-bit Bounce Buffer (SWIOTLB)]
    Bounce <-->|CPU memcpy| RAM
```

---

## 1. Coherent vs Streaming DMA

- **Coherent (Consistent) DMA**: Cache-coherent memory where CPU writes are immediately visible to hardware without explicit flush operations.
- **Streaming DMA**: Buffers mapped for single I/O operations requiring memory barriers (`dma_sync_single_for_cpu` / `dma_sync_single_for_device`).
