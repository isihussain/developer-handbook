# Consensus Architecture: Viewstamped Replication Revisited

Viewstamped Replication (VRR), introduced by Oki and Liskov, predates Paxos and Raft, providing clean primary-backup consensus based on explicitly numbered views.

```mermaid
flowchart LR
    Client -->|Request| Primary[Primary: View v]
    Primary -->|Prepare v, n, d| Backup1[Backup 1]
    Primary -->|Prepare v, n, d| Backup2[Backup 2]
    Backup1 -->|PrepareOK v, n, i| Primary
    Backup2 -->|PrepareOK v, n, i| Primary
    Primary -->|Commit v, n| Client
```

---

## 1. Primary Components
- **View-Number:** Monotonic identifier of the current primary tenure.
- **Op-Number:** Sequential order of operations in the log.
- **Commit-Number:** Highest op-number known to be replicated on a quorum.
