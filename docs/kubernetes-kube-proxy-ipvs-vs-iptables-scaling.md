# Kubernetes Networking: IPVS vs iptables Performance Scaling

When clusters exceed 5,000 services, sequential iptables rule chains introduce $O(N)$ packet lookup latency. IPVS provides $O(1)$ IP hash table routing.

```mermaid
flowchart LR
    Packet[Packet destined to Service ClusterIP] --> Mode{kube-proxy Mode}
    Mode -->|iptables: O(N)| Chains[Sequential KUBE-SVC-* chains: CPU spike on rule update!]
    Mode -->|IPVS: O(1)| Hash[IPVS Kernel Hash Table: Microsecond lookup at 50,000 Services!]
    Hash --> Pod[Destination Pod IP]
```

---

## 1. Connection Balancing
IPVS supports advanced scheduler algorithms:
- `rr` (Round Robin)
- `lc` (Least Connection)
- `dh` (Destination Hashing)
- `sed` (Shortest Expected Delay)
