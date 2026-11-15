# Linux Kernel: sk_buff Memory Architecture & Linearization Costs

The Linux networking stack manages packet buffers using `struct sk_buff`. Packet clones share read-only data payloads, while `skb_linearize()` reassembles paged fragments into contiguous memory.

```mermaid
flowchart TD
    subgraph sk_buff[struct sk_buff: Control Structure]
        head[head: Start of buffer]
        data[data: Start of active packet protocol layer]
        tail[tail: End of active packet payload]
        end_ptr[end: End of allocated buffer memory]
    end
    subgraph shinfo[struct skb_shared_info]
        nr_frags[nr_frags: Number of page fragments]
        frags[frags[MAX_SKB_FRAGS]: Non-contiguous 4KB pages in RAM]
        dataref[dataref: Atomic reference count]
    end
    end_ptr --> shinfo
```

---

## 1. Cloning vs Copying & Linearization

* **`skb_clone()`:** Creates a new `sk_buff` header pointing to the same existing data buffer; increments `dataref`. Modifying payload data requires `skb_copy_expand()` to avoid race conditions.
* **Non-Contiguous Fragments (`skb_shinfo`):** High-speed NICs use scatter-gather DMA to deposit jumbo packet payloads across disjoint memory page fragments.
* **`skb_linearize()` Penalty:** If a legacy network driver requires a flat contiguous memory buffer, the kernel must allocate a new buffer and copy all fragments, incurring CPU latency and cache eviction penalties.
