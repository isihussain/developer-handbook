# Raft Consensus: Joint Consensus Dynamic Membership Changes

Raft's joint consensus configuration transition allows a cluster to transition between two distinct membership configurations (`C_old` and `C_new`) without any period where split-brain or two independent quorums can form.

```mermaid
sequenceDiagram
    participant Leader
    participant Quorum_Old as C_old Nodes
    participant Quorum_New as C_new Nodes

    Leader->>Quorum_Old: Propose Entry with C_old,new
    Leader->>Quorum_New: Propose Entry with C_old,new
    Note over Leader,Quorum_New: Configuration decisions now require independent majorities from BOTH C_old and C_new
    Quorum_Old-->>Leader: Majority Ack from C_old
    Quorum_New-->>Leader: Majority Ack from C_new
    Note over Leader: C_old,new is committed!
    Leader->>Quorum_Old: Propose Entry with C_new
    Leader->>Quorum_New: Propose Entry with C_new
    Quorum_New-->>Leader: Majority Ack from C_new
    Note over Leader: C_new committed. Old nodes decommissioned.
```

---

## 1. Safety Invariants
1. **No Disjoint Majorities:** Any majority of `C_old` intersects with any other majority of `C_old`. Similarly, any majority of `C_new` intersects with any majority of `C_new`.
2. **Immediate Effect:** Each node begins using the new configuration as soon as it appends the entry to its local log, regardless of whether it is committed yet.
3. **Commit Rule:** A log entry under joint consensus requires independent majorities from both `C_old` and `C_new` before being marked committed.
