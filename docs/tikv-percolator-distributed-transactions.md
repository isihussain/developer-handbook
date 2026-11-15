# TiKV: Percolator Distributed Transaction Model

TiKV implements ACID snapshot isolation across distributed key-value storage nodes using Google's Percolator two-phase commit protocol, coordinated by a Placement Driver (PD).

```mermaid
sequenceDiagram
    participant Client
    participant PD as Placement Driver (Timestamp Oracle)
    participant Primary as Primary Key Shard
    participant Secondary as Secondary Key Shard
    Client->>PD: Get start_ts
    PD-->>Client: start_ts = 100
    Client->>Primary: Prewrite(primary_key, lock, start_ts=100)
    Client->>Secondary: Prewrite(secondary_key, lock->primary, start_ts=100)
    Client->>PD: Get commit_ts
    PD-->>Client: commit_ts = 105
    Client->>Primary: Commit(primary_key, commit_ts=105)
    Note over Client,Secondary: Secondary keys commit asynchronously
    Client->>Secondary: Roll-forward commit
```

---

## 1. Three Column Families

* **`data` CF:** Stores actual row values indexed by key and commit timestamp `(key, commit_ts)`.
* **`lock` CF:** Holds uncommitted locks pointing to the primary key lock `(key, start_ts)`.
* **`write` CF:** Records committed metadata `(key, commit_ts) -> start_ts` for MVCC reads.
