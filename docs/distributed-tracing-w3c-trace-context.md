# Distributed Tracing: W3C Trace Context Protocol

W3C Trace Context standardizes HTTP header formats (`traceparent` and `tracestate`) to propagate distributed transaction traces across heterogeneous microservice networks.

```mermaid
flowchart LR
    Client -->|traceparent: 00-4bf92f3577b34da6a3ce929d0e0e4736-00f067aa0ba902b7-01| ServiceA[Frontend Gateway]
    ServiceA -->|Generates new span_id, forwards trace_id| ServiceB[Order Service]
    ServiceB -->|Propagates parent span| ServiceC[Payment Service]
```

---

## 1. `traceparent` Header Specification

`version - trace_id - parent_id / span_id - trace_flags`
* **Version (2 hex):** Current specification standard is `00`.
* **Trace ID (32 hex):** 16-byte globally unique identifier for the distributed transaction.
* **Parent ID (16 hex):** 8-byte identifier of the incoming parent span.
* **Trace Flags (2 hex):** Bitmask controlling recording options (`01` indicates sampled).
