# Distributed SQL: CockroachDB Multi-Region Range Lease Placement

CockroachDB partitions tables into 64MB key ranges. Multi-region survivability and low latency are achieved by anchoring Range Leases close to users.

```mermaid
flowchart TD
    ClientEU[European User Query] --> GatewayEU[CockroachDB Gateway (Frankfurt)]
    GatewayEU --> LeaseCheck{Is Range Lease in Europe?}
    LeaseCheck -->|Yes: REGIONAL BY ROW| LocalRead[Serve Read Locally in 2ms!]
    LeaseCheck -->|No| CrossRegion[Cross-Atlantic RPC: 100ms latency!]
```

---

## 1. Table Localities
- `REGIONAL BY TABLE`: Anchors entire table to a specific region.
- `REGIONAL BY ROW`: Dynamically places individual rows and their leases in the user's home region based on a region column.
- `GLOBAL`: Replicates reads locally everywhere via non-blocking multi-version timestamps.
