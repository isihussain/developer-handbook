# Message Broker Architecture: RabbitMQ Quorum Queues (Raft)

RabbitMQ Quorum Queues replace deprecated mirrored classic queues with the Raft consensus algorithm, guaranteeing data safety and predictable recovery.

```mermaid
flowchart TD
    Publisher[Publisher: Publish with publisher-confirms] --> Leader[Quorum Queue Leader Node]
    Leader -->|Raft Log Replication| Follower1[Raft Follower Node 1]
    Leader -->|Raft Log Replication| Follower2[Raft Follower Node 2]
    Follower1 -->> Leader: Ack
    Leader -->> Publisher: Basic.Ack (Quorum Majority Confirmed!)
```

---

## 1. Split-Brain Safety
Classic mirrored queues suffered silent data loss during network partitions when nodes unsynchronized. Quorum queues use formal Raft consensus, strictly preventing split-brain writes.
