# Event Driven Architecture: Redis Streams Pending Entries List (PEL)

Redis Streams (`XADD`, `XREADGROUP`) provides Kafka-like consumer group offset tracking and message acknowledgment.

```mermaid
flowchart TD
    Stream[Redis Stream: 'orders'] --> Group[Consumer Group: 'fulfillment']
    Group --> Read[XREADGROUP: Consumer 1 reads Message ID 16000-0]
    Read --> PEL[Message moved to Pending Entries List (PEL)]
    PEL --> Ack{XACK received?}
    Ack -->|Yes| Remove[Removed from PEL: Complete]
    Ack -->|No / Consumer Crashes| XCLAIM[XCLAIM / XAUTOCLAIM: Transferred to Consumer 2 after timeout!]
```

---

## 1. Dead Letter Handling
The PEL tracks idle time and delivery count (`delivery_count`). When attempts exceed a threshold, messages are moved to a dead-letter queue.
