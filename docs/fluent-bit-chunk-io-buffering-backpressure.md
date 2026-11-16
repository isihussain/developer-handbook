# Observability Pipelines: Fluent Bit Chunk I/O and Memory Queues

Fluent Bit processes millions of log events per second with low memory footprints, using a hybrid in-memory and filesystem storage buffering engine.

```mermaid
flowchart TD
    Input[Input Plugin: Tail container logs] --> MemoryBuffer[In-Memory Buffer]
    MemoryBuffer --> CheckLimit{"Memory > Mem_Buf_Limit?"}
    CheckLimit -->|Yes| DiskBuffer[Filesystem Buffer: Spill chunks to disk (.flb)]
    CheckLimit -->|No| Filter[Filter Engine: Grep, Parser, Lua]
    Filter --> Output[Output: Elasticsearch / S3 / Loki]
    Output -->|Upstream Slow / 429| Pause[Pause Input Ingestion: Zero Log Loss!]
```

---

## 1. Backpressure Mitigation
Configuring `storage.type filesystem` prevents OOM crashes when log ingestion spikes or downstream destinations suffer network outages.
