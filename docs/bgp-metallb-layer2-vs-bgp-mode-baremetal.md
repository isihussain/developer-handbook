# Bare Metal Networking: MetalLB Load Balancing Architecture

MetalLB assigns external IP addresses to Kubernetes LoadBalancer services on bare-metal clusters without cloud provider load balancers.

```mermaid
flowchart TD
    Client[External Client Traffic] --> Mode{MetalLB Operating Mode}
    Mode -->|Layer 2 Mode| LeaderNode[Single Leader Node answers ARP/NDP: Node is single-point bottleneck!]
    Mode -->|BGP Mode| Router[Physical BGP Top-of-Rack Router]
    Router --> ECMP[ECMP: Equal-Cost Multi-Path distributes traffic across ALL nodes!]
    ECMP --> Node1[Node 1]
    ECMP --> Node2[Node 2]
```

---

## 1. BGP Mode Advantage
True load balancing at line rate with zero single-node bottleneck and instant failover via BGP peer keepalive timeouts.
