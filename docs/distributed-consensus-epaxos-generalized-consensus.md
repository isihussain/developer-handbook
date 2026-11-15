# Distributed Consensus: Egalitarian Paxos (EPaxos) & Fast-Path Quorums

Egalitarian Paxos eliminates master-leader bottlenecks by allowing any replica to propose and commit commands, dynamically resolving order dependencies through explicit dependency graphs.

```mermaid
flowchart TD
    Client1[Client A] -->|Propose x| Replica1[Replica 1: Command Leader for x]
    Client2[Client B] -->|Propose y| Replica2[Replica 2: Command Leader for y]
    Replica1 -->|Fast-Path: 1 RTT if no dependency conflict| Commit1[Fast-Path Commit: 1 RTT]
    Replica2 -->|Slow-Path: 2 RTT if concurrent conflicting keys detected| Resolve[Dependency Resolution Phase]
    Resolve --> Commit2[Slow-Path Commit: 2 RTT]
```

---

## 1. Quorums & Dependency Graphs

1. **Fast-Path Quorum:** Size $F = f + \lfloor rac{f + 1}{2} floor$; if all replicas in the fast quorum agree on identical dependencies, the command commits in one round-trip.
2. **Conflict Detection:** Commands that do not commute (accessing identical state keys) record mutual dependencies ($x 	o y$ or $y 	o x$).
3. **Execution Phase:** Replicas compute topological sorting over the strongly connected components of the dependency graph, executing commands identically across all nodes.
