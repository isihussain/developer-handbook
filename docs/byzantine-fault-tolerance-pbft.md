# Practical Byzantine Fault Tolerance (PBFT)

PBFT achieves consensus in asynchronous distributed systems subject to Byzantine (arbitrary/malicious) failures.

```mermaid
sequenceDiagram
    autonumber
    Client->>Primary: Request(m)
    Primary->>Backups: Pre-Prepare(view, seq, digest)
    Backups->>All: Prepare(view, seq, digest, backup_id)
    Note over Primary,Backups: 2f + 1 Prepare messages received
    All->>All: Commit(view, seq, digest, node_id)
    Note over Primary,Backups: 2f + 1 Commit messages received
    Backups-->>Client: Reply(result)
```

---

## 1. Bound $3f + 1$

To tolerate $f$ Byzantine nodes:
$$N \ge 3f + 1$$
Guarantees quorum intersection between any two sets of $2f + 1$ nodes includes at least $f + 1$ honest nodes.
