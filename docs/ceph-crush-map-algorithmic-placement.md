# Distributed Object Storage: Ceph CRUSH Map Placement Algorithm

Ceph eliminates centralized metadata servers (lookup tables) for object placement by calculating storage node locations deterministically using CRUSH (Controlled Replication Under Scalable Hashing).

```mermaid
flowchart TD
    ObjectID["Object: 'photo_992'"] --> Hash["Hash(ObjectID) = 0x8A72F1B0"]
    Hash --> PG["Placement Group: PG_4.12"]
    PG --> CRUSH["CRUSH Map Ruleset: (Fault Domain: Rack)"]
    CRUSH --> OSD1["OSD 14 (Rack A)"]
    CRUSH --> OSD2["OSD 28 (Rack B)"]
    CRUSH --> OSD3["OSD 42 (Rack C)"]
```

---

## 1. Hierarchical Cluster Map
CRUSH maps model real-world topology:
$$\text{Root} \rightarrow \text{Datacenter} \rightarrow \text{Row} \rightarrow \text{Rack} \rightarrow \text{Host} \rightarrow \text{OSD}$$
Replicas are placed across isolated fault domains to survive rack/power failure.
