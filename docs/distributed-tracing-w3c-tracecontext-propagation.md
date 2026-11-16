# Distributed Observability: W3C TraceContext Specification

W3C TraceContext standardizes distributed tracing HTTP headers across polyglot microservice boundaries.

```mermaid
sequenceDiagram
    participant Frontend
    participant ServiceA as Service A
    participant ServiceB as Service B

    Frontend->>ServiceA: traceparent: 00-4bf92f3577b34da6a3ce929d0e0e4736-00f067aa0ba902b7-01
    Note over ServiceA: Parses: version(00) - trace_id - parent_id - trace_flags(01 sampled)
    Note over ServiceA: Generates new span_id: 5fb397be34d23b0f
    ServiceA->>ServiceB: traceparent: 00-4bf92f3577b34da6a3ce929d0e0e4736-5fb397be34d23b0f-01
    Note over ServiceB: trace_id remains identical across entire distributed lifecycle!
```

---

## 1. The `tracestate` Header
Carries vendor-specific opaque routing tags (e.g. AWS X-Ray, Datadog, Jaeger) alongside the standard W3C identifier.
