# Distributed Tracing: Grafana Tempo and Parquet Storage

Grafana Tempo delivers massive trace ingestion at low cost by eliminating distributed search indexes (Elasticsearch/Cassandra) in favor of object storage (S3/GCS) with Apache Parquet.

```mermaid
flowchart LR
    TracesIn[OTLP Trace Spans] --> Distributor[Distributor: Hash-rings spans by TraceID]
    Distributor --> Ingester[Ingester: Buffers spans into Parquet Blocks]
    Ingester -->|Flush| S3[(Object Storage: S3 / GCS / Azure Blob)]
    Querier[Querier: TraceID Lookup via Parquet Bloom Filters] --> S3
```

---

## 1. Cost Efficiency
Relying entirely on commodity object storage reduces total cost of ownership by up to 80% compared to traditional indexed trace backends.
