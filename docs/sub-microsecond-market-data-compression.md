# Sub-Microsecond Market Data Compression Architecture

Techniques for compressing financial tick data using bit-packing, delta encoding, and SIMD without decompression penalties.

```mermaid
flowchart LR
    Prices[Price Series: 10001, 10002, 10000, 10003] --> Delta[Delta Encoding: +1, -2, +3]
    Delta --> BitPack[Bit-Packing: 4 bits per delta instead of 64 bits]
    BitPack --> Wire[3.2x Compression Ratio on 10G/25G Links]
```

---

## 1. Delta-of-Delta Bit-Packing

Financial prices exhibit temporal locality; consecutive updates typically differ by only a few price ticks. Storing delta-of-deltas eliminates redundant higher-order bits:

```cpp
#include <cstdint>

struct CompressedTick {
    uint32_t timestamp_delta_nanos : 16;
    int32_t price_delta_ticks : 8;
    int32_t volume_delta : 8;
};
static_assert(sizeof(CompressedTick) == 4, "Compressed tick must fit in 32 bits");
```
