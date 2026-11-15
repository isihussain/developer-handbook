# Raft Consensus: Log Compaction & Snapshotting Architecture

Raft prevents write logs from growing unbounded by periodically capturing state machine snapshots, discarding preceding log entries and streaming snapshot chunks to catch up lagging followers.

```mermaid
flowchart TD
    Log[Continuous Raft Log: Entries 1 .. 100000] --> Snap[Capture State Machine Snapshot at Index 95000]
    Snap --> Discard[Discard Entries 1 .. 95000 from Disk Log]
    Discard --> Meta[Store Metadata: last_included_index = 95000, last_included_term = 4]
    Follower[Lagging Follower at Index 4000] -->|Leader sends InstallSnapshot RPC| Catchup[Catch Up via Snapshot Stream]
```

---

## 1. InstallSnapshot RPC Invariant

When leader discovers a follower's `nextIndex` precedes the leader's oldest retained log entry, it bypasses `AppendEntries` and transmits the snapshot image directly using `InstallSnapshot`.
