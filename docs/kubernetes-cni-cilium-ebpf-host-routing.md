# Cloud Native Networking: Cilium eBPF Host Routing

Cilium bypasses the host iptables and IP routing stack entirely, routing packets between containers and NICs directly inside the kernel via eBPF.

```mermaid
flowchart TD
    PodA[Pod A Container Network Namespace] --> Veth[veth pair / IPVLAN]
    Veth --> BPF_Ingress[eBPF Program attached to tc / xdp hook]
    BPF_Ingress --> MapLookup[Cilium BPF Endpoint & Service Maps]
    MapLookup --> DirectRedirect[bpf_redirect_peer(): Zero Netfilter Overhead!]
    DirectRedirect --> PodB[Pod B Container Network Namespace]
```

---

## 1. Performance Gains
- Eliminates context switches between socket layer and TCP/IP stack.
- Achieves bare-metal throughput and sub-millisecond latencies for east-west traffic.
