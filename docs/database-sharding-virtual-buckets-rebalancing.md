# Distributed Data: Virtual Buckets Sharding Architecture

Direct physical sharding makes rebalancing painful. Virtual bucketing (used in Couchbase and DynamoDB) decouples logical keyspaces from physical nodes.

```mermaid
flowchart TD
    Key[User Key: 'user_459'] --> Hash["CRC32(Key) % 1024 = Bucket 87"]
    BucketMap[Bucket-to-Node Routing Table] --> Lookup["Bucket 87 -> Node B"]
    Lookup --> NodeB[Node B Storage Engine]
    subgraph Rebalancing
        NodeAdd[Add New Node C] --> MoveBucket["Move only Buckets 80-120 from Node B to C without rehashing keys!"]
    end
```

---

## 1. Constant Shard Count
The total number of virtual buckets (e.g. 1,024) remains static for the lifetime of the cluster, eliminating table-wide data rewrites when physical servers scale.
