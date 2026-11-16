# Cloud-Scale Metrics: Grafana Mimir Architecture

Grafana Mimir provides horizontally scalable, multi-tenant long-term storage for Prometheus metrics, capable of ingesting 1 billion active series.

```mermaid
flowchart TD
    MetricsIn[Prometheus Remote Write] --> Distributor[Distributor: Shards series across Ingesters]
    Distributor --> IngesterRing[Ingester Ring: Replication Factor 3]
    IngesterRing --> Flush[Flush TSDB Blocks to Object Storage]
    Flush --> S3[(Object Storage: S3 / GCS)]
    Querier[Querier + Query Frontend: Splits queries by time and sub-queries] --> S3
```

---

## 1. Query Splitting & Caching
The Query Frontend splits large time range queries into 1-day sub-queries, executes them in parallel across worker pools, and caches immutable results.
