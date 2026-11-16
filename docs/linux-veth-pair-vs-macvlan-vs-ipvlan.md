# Linux Networking: Container Virtual Interfaces Comparison

Container network plugins (CNIs) choose between virtual ethernet pairs, macvlan, and ipvlan based on performance and MAC table limits.

```mermaid
flowchart TD
    PhysicalNIC[Physical Host NIC: eth0] --> Choice{Virtualization Mode}
    Choice -->|veth pair| Bridge[Linux Bridge: Packet traverses TCP stack twice: High Overhead]
    Choice -->|macvlan| MAC[Each Container gets unique MAC address: Fast, but exhausts switch MAC tables!]
    Choice -->|ipvlan L2/L3| IP[Containers share Host MAC address: L3 mode routes directly inside kernel!]
```

---

## 1. IPVLAN L3 Advantage
Eliminates ARP broadcast storms and MAC table limits on underlying datacenter switches while delivering near-native bare-metal packet performance.
