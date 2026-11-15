# High-Performance Networking: eBPF XDP_REDIRECT & Devmap Forwarding

eBPF XDP (eXpress Data Path) achieves ultra-high packet forwarding rates by evaluating filtering and routing logic at the NIC driver layer, redirecting frames across physical interfaces via `BPF_MAP_TYPE_DEVMAP`.

```mermaid
flowchart LR
    Ingress[NIC eth0: RX Ring] --> XDP[XDP Program: eBPF bytecode]
    XDP --> Check{Parsing & Routing Logic}
    Check -->|Lookup target port| Devmap[BPF_MAP_TYPE_DEVMAP: eth1]
    Devmap -->|bpf_redirect_map(..., XDP_REDIRECT)| TX_Ring[NIC eth1: TX Ring]
    TX_Ring --> Wire[Line Rate Egress: Bypasses sk_buff allocation]
```

---

## 1. Performance Characteristics

1. **Zero `sk_buff` Allocation:** Packets are processed directly in Driver RX page buffers without allocating kernel `struct sk_buff` descriptors.
2. **Devmap Batching:** `bpf_redirect_map()` queues packets into per-CPU bulk queues, flushing them in batches to target NIC TX rings to saturate 100GbE line rates.
3. **Multi-Interface Bridging:** Enables software routers, firewalls, and DDoS mitigation appliances to forward millions of packets per second (Mpps) per core.
