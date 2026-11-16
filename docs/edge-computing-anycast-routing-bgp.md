# Edge Acceleration: BGP Anycast Routing Architecture

Anycast advertises the exact same public IP block (e.g. `1.1.1.1`) from hundreds of globally distributed edge data centers simultaneously.

```mermaid
flowchart TD
    ClientEurope[Client in Frankfurt] -->|Shortest BGP AS-Path: 2ms| EdgeFRA[Cloudflare Frankfurt PoP]
    ClientAsia[Client in Tokyo] -->|Shortest BGP AS-Path: 3ms| EdgeTYO[Cloudflare Tokyo PoP]
    EdgeFRA --> Backbone[Private Global Fiber Backbone]
    EdgeTYO --> Backbone
    Backbone --> Origin[Central Origin Server]
```

---

## 1. DDoS Resilience
Volumetric attacks are naturally partitioned across regional Points of Presence (PoPs), preventing core origin saturations.
