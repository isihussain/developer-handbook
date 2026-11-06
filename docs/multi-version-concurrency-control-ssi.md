# Serializable Snapshot Isolation (SSI) Architecture

SSI (Cahill et al., PostgreSQL 9.1+) provides true Serializable isolation without pessimistic read locks.

```mermaid
graph LR
    T1[Transaction 1] -->|rw-antidependency| T2[Transaction 2]
    T2 -->|rw-antidependency| T3[Transaction 3]
    T3 -.->|Cycle detected in Serialization Graph| Abort[Abort T3 (First-Committer-Wins)]
```

---

## 1. rw-Antidependencies (`siREAD` Locks)

- A rw-antidependency occurs when transaction $T_1$ reads a version of data, and concurrent transaction $T_2$ writes a newer version of that same data.
- SSI tracks these dependencies using non-blocking in-memory `siREAD` locks.
- A serialization anomaly requires two consecutive rw-antidependency edges in the dependency graph. SSI aborts the first transaction attempting to commit when a dangerous cycle forms.
