# Linux Network Stack: struct sk_buff Memory Architecture

The `sk_buff` (socket buffer) represents a network frame traversing the kernel, structured with headroom and tailroom to avoid memory reallocations during packet encapsulation.

```mermaid
flowchart LR
    subgraph SKB_Memory[sk_buff Contiguous Memory Buffer]
        Head[Headroom: Protocol Headers Reserved Space]
        Data[Actual Payload Data]
        Tail[Tailroom: Trailer & Padding Space]
    end
    Head -->|skb_push: Prepend IP Header| Head
    Tail -->|skb_put: Append CRC| Tail
```

---

## 1. Cloning vs Copying
- `skb_clone()`: Copies only the `sk_buff` metadata descriptor while sharing the packet payload buffer (using reference counting).
- `skb_copy()`: Fully duplicates both descriptor and underlying packet payload.
