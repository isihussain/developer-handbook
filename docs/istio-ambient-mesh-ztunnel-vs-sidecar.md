# Service Mesh Architecture: Istio Ambient ztunnel vs Sidecars

Ambient Mesh splits Layer 4 transport security (mTLS) from Layer 7 application routing, replacing per-pod Envoy sidecars with a shared node proxy (`ztunnel`).

```mermaid
flowchart TD
    Pod[Application Pod: No Envoy Sidecar!] --> Ztunnel[ztunnel: Node-Level L4 Proxy in Rust]
    Ztunnel -->|HBONE: HTTP-Based Overlay Network Encapsulation| PeerZtunnel[Destination Node ztunnel]
    PeerZtunnel --> DestPod[Destination Pod]
    subgraph OptionalL7
        Ztunnel -.-> Waypoint[Dedicated Waypoint Proxy (Envoy) for L7 WAF/Auth]
    end
```

---

## 1. Resource Savings
- Eliminates 50MB-100MB RAM overhead per container pod.
- Application pods can be upgraded or restarted without interrupting the service mesh data plane.
