# Cloud Messaging: NATS JetStream Distributed Storage

NATS JetStream adds persistence, deduplication, and KV/object storage to lightweight pub/sub using Raft consensus.

```mermaid
flowchart TD
    Publish[NATS Client Publish] --> JetStream[JetStream Meta Controller]
    JetStream --> RaftGroup[Raft Consensus Clustered Storage: RF 3]
    RaftGroup --> Disk[Append-Only Message Store on NVMe]
    Disk --> Ack[Return PubAck to Producer]
```

---

## 1. Memory vs File Streams
Streams can be configured as `File` (durable against node crashes) or `Memory` (ultra-fast in-memory replicated buffer).
