# Cut-Through vs Store-and-Forward Switching Latency Architecture

Cut-through Ethernet switches begin forwarding frames immediately after parsing the destination MAC address (first 64 bytes), reducing packet forwarding latency from microseconds down to sub-150 nanoseconds.

```mermaid
flowchart TD
    subgraph Cut-Through Switching: Latency ~ 120ns
        P1[Read Preamble & Destination MAC: 14 bytes] --> Decision[Lookup Forwarding Table]
        Decision --> Forward[Immediately start transmitting frame on egress port]
        Forward -.-> Tail[Packet body & FCS still arriving on wire]
    end
    subgraph Store-and-Forward: Latency ~ 1.5us to 10us
        P2[Receive entire packet payload into memory] --> CRC[Compute & Validate Frame Check Sequence FCS]
        CRC --> Verify[If valid, transmit on egress port]
    end
```

---

## 1. Latency Formulation

* **Store-and-Forward Latency:**
  $$L_{	ext{store}} = rac{	ext{Packet Size (bits)}}{	ext{Port Bandwidth (bps)}} + L_{	ext{internal}}$$
* **Cut-Through Latency:**
  $$L_{	ext{cut}} = rac{64 	ext{ bytes} 	imes 8}{	ext{Port Bandwidth (bps)}} + L_{	ext{internal}}$$
  (Independent of total packet frame size).
