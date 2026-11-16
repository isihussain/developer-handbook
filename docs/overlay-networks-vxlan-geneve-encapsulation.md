# Cloud Networking: VXLAN and GENEVE Overlay Encapsulation

Overlay networks allow multi-tenant Layer 2 Ethernet frames to be encapsulated inside Layer 3 UDP datagrams traversing standard datacenter underlay switches.

```mermaid
flowchart LR
    InnerFrame[Original L2 Ethernet Frame] --> VXLAN_Header[VXLAN Header: 24-bit VNI (16 Million Segments)]
    VXLAN_Header --> UDP[Outer UDP Header: Port 4789]
    UDP --> IP[Outer IP Header: VTEP Source & Dest]
    IP --> PhysicalWire[Underlay Physical Network]
```

---

## 1. MTU Consideration
Encapsulation adds 50 bytes (VXLAN) or up to 64 bytes (GENEVE). Network interfaces must support jumbo frames (MTU 9000) or reduce container MTU to 1450 to prevent fragmentation.
