# Solarflare ef_vi Low-Level Driver Architecture

Solarflare `ef_vi` (EtherFabric Virtual Interface) provides direct access to network card DMA descriptor rings, bypassing socket abstractions entirely.

```mermaid
flowchart LR
    App[Trading Application] -->|Direct MMIO Doorbell| HardwareNIC[Solarflare SFN8522 NIC]
    HardwareNIC -->|DMA Write into Registered Packet Memory| HostDRAM[Host Hugepages]
    App -->|Poll Event Queue ef_eventq_poll| HostDRAM
```

---

## 1. Key ef_vi Primitives

- `ef_vi_alloc`: Allocates a virtual interface containing dedicated TX/RX descriptor rings and event queues.
- `ef_memreg_alloc`: Registers hugepages with the NIC hardware for direct zero-copy DMA access.
- `ef_vi_receive_post`: Posts a buffer address to the RX ring.
- `ef_eventq_poll`: Polls the event queue directly from CPU cache with sub-microsecond latency.

---

## 2. Ingress Polling Loop with ef_vi

```c
#include <etherfabric/vi.h>
#include <etherfabric/pd.h>

void ef_vi_polling_loop(ef_vi* vi) {
    ef_event evs[32];
    while (1) {
        int n_ev = ef_eventq_poll(vi, evs, 32);
        for (int i = 0; i < n_ev; ++i) {
            if (EF_EVENT_TYPE(evs[i]) == EF_EVENT_TYPE_RX) {
                ef_addr dma_addr = EF_EVENT_RX_DMA_ADDR(evs[i]);
                uint8_t* pkt = (uint8_t*)ef_vi_dma_to_va(vi, dma_addr);
                int len = EF_EVENT_RX_BYTES(evs[i]);

                process_packet(pkt, len);

                // Repost buffer to RX ring
                ef_vi_receive_post(vi, dma_addr, 0);
            }
        }
    }
}
```
