# Byzantine Consensus: Practical Byzantine Fault Tolerance (PBFT)

Castro and Liskov's PBFT algorithm achieves state machine replication in asynchronous environments vulnerable to malicious or arbitrary Byzantine failures, tolerating up to $f$ faulty nodes in a system of $3f + 1$ replicas.

```mermaid
sequenceDiagram
    participant Client
    participant Primary as Primary Leader (Replica 0)
    participant Backup1 as Backup Replica 1
    participant Backup2 as Backup Replica 2
    Client->>Primary: Request (op, timestamp, c)
    Primary->>Backup1: Pre-Prepare(v, n, d)
    Primary->>Backup2: Pre-Prepare(v, n, d)
    Backup1->>Primary: Prepare(v, n, d, i)
    Backup1->>Backup2: Prepare(v, n, d, i)
    Note over Backup1,Backup2: 2f + 1 Prepare messages received -> Prepared state
    Backup1->>Primary: Commit(v, n, d, i)
    Backup1->>Backup2: Commit(v, n, d, i)
    Note over Backup1,Backup2: 2f + 1 Commit messages received -> Committed state
    Backup1-->>Client: Reply(v, t, c, i, r)
    Backup2-->>Client: Reply(v, t, c, i, r)
    Note over Client: Awaits f + 1 identical valid replies
```

---

## 1. Protocol Invariants

* **Quorum Requirements:** Total nodes $R \ge 3f + 1$; quorum threshold for both Prepare and Commit phases is $2f + 1$ valid signatures.
* **View Change:** If the primary leader stalls or behaves dishonestly, backups timeout and broadcast `VIEW-CHANGE` messages to elect primary $(v + 1) \pmod R$.
