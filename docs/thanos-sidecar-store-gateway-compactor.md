# Long-Term Metric Storage: Thanos Architecture

Thanos scales Prometheus horizontally to multi-year retention across multi-cluster environments using object storage.

```mermaid
flowchart TD
    Prometheus[Prometheus Instance] --> Sidecar[Thanos Sidecar: Ships 2h TSDB blocks to S3]
    Sidecar --> S3[(Object Storage: S3 Bucket)]
    S3 --> StoreGateway[Thanos Store Gateway: Serves historical queries from S3]
    S3 --> Compactor[Thanos Compactor: Downsamples data to 5m and 1h resolutions]
    Query[Thanos Query / Grafana] --> StoreGateway
    Query --> Sidecar
```

---

## 1. Downsampling Optimization
Compactor aggregates historical metrics into 5-minute and 1-hour resolution chunks, allowing multi-year dashboard queries to render in milliseconds.
