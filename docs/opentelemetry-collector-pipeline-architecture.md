# Observability: OpenTelemetry Collector Pipeline Architecture

The OpenTelemetry Collector provides vendor-agnostic ingestion, transformation, and routing for distributed traces, metrics, and logs.

```mermaid
flowchart LR
    Receivers[Receivers: OTLP gRPC/HTTP, Jaeger, Prometheus] --> Processors[Processors: Batch, Memory Limiter, Transform, Filter]
    Processors --> Exporters[Exporters: OTLP, Datadog, Prometheus, Kafka]
    subgraph Data_Flow
        Receivers -->|In-memory Buffer| Processors
        Processors -->|Batched Chunks| Exporters
    end
```

---

## 1. Memory Limiter Processor
Mandatory in production: Monitors heap usage and drops or rejects incoming telemetry with backpressure when consumption approaches hard container limits.
