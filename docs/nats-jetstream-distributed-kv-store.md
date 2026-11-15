# NATS JetStream: Distributed Key-Value Store Internals

NATS JetStream embeds a distributed key-value store directly into its pub/sub messaging broker using stream consumers and lightweight Raft consensus groups.

```mermaid
flowchart LR
    Client[KV Client] -->|Bucket Put: kv.bucket.key| JetStream[JetStream Engine]
    JetStream --> Stream["Stream: KV_bucket (File-backed Log)"]
    Stream --> Rollup["Subject Rollup: Discards older versions per key"]
    Rollup --> ClusterReplicas[Raft Quorum: R=3 / R=5]
```

---

## 1. Implementation Characteristics

* **Streams as Key-Value Stores:** Keys map to stream subjects (`$KV.<bucket>.<key>`). Each put appends a message; updates trigger subject rollups to retain the latest value.
* **Watch API via Subscriptions:** Watch operations map to standard NATS message subscriptions, delivering sub-millisecond change notifications without polling.
