# Database Engines: Inverted Index Compression with Roaring Bitmaps

Roaring Bitmaps partition 32-bit integer sets into chunks of $2^{16}$ (65,536) integers, dynamically choosing optimal container structures based on item density.

```mermaid
flowchart TD
    Int32[32-bit Integer: e.g. 0x0002_001F] --> Split[Split into High 16 bits & Low 16 bits]
    Split --> High16["High 16 bits (0x0002): Indexes Chunk Directory"]
    Split --> Low16["Low 16 bits (0x001F): Stored in Container"]
    High16 --> ContainerType{Item Density in Chunk}
    ContainerType -->|< 4096 Integers| ArrayContainer[Array Container: Sorted uint16 array]
    ContainerType -->|>= 4096 Integers| BitContainer[Bitmap Container: Flat 8KB Bitset]
    ContainerType -->|Consecutive Runs| RunContainer[Run Container: Array of [start, length] pairs]
```

---

## 1. Container Formats & SIMD Set Operations

* **Array Container:** Stores up to 4,095 values as raw 16-bit integers ($< 8	ext{KB}$ memory footprint).
* **Bitmap Container:** Exactly 8,192 bytes ($2^{16}$ bits), where bit index corresponds directly to integer presence.
* **SIMD Bitwise Boolean Operations:** Intersecting two bitmap containers executes via 512-bit vector bitwise instructions (`VPANDQ` / `VPORQ`), evaluating tens of millions of records per millisecond.
