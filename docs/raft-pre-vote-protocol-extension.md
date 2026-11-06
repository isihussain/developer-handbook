# Raft Consensus: Pre-Vote Protocol Extension

The Pre-Vote extension prevents partitioned nodes from disrupting stable Raft clusters upon network reconnection.

```mermaid
sequenceDiagram
    autonumber
    Note over PartitionedNode: Partitioned node times out
    PartitionedNode->>Peers: PreVote(term, candidateId, lastLogIndex, lastLogTerm)
    Note over Peers: Peers check if current leader is active
    Peers-->>PartitionedNode: Reject PreVote (Leader heartbeat active)
    Note over PartitionedNode: Node stays Follower; term NOT incremented
```

---

## 1. The Disrupted Leader Problem

Without Pre-Vote:
- A partitioned node repeatedly times out, incrementing its `currentTerm` ($T+1, T+2, T+50$).
- When the partition heals, the node broadcasts `RequestVote` with term $T+50$.
- The healthy leader discovers a higher term and immediately steps down, forcing an unnecessary cluster-wide election.

---

## 2. Pre-Vote Phase Fix

Before incrementing its term, a candidate enters state `PreCandidate` and queries peers:
- Peers only vote for a `PreCandidate` if they haven't received a heartbeat from a valid leader within the election timeout.
