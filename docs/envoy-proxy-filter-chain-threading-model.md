# Service Mesh Architecture: Envoy Threading & Filter Pipeline

Envoy operates a single-process, multi-threaded event-driven architecture using `libevent`, where each worker thread runs an independent non-blocking event loop.

```mermaid
flowchart TD
    MainThread[Main Thread: Control Plane / xDS Ingestion] --> SharedConfig[Thread-Local Storage TLS Config]
    Worker1[Worker Thread 1: Event Loop] --> Downstream[Downstream TCP Connection]
    Downstream --> ListenerFilter[Listener Filters: e.g. TLS Inspector]
    ListenerFilter --> NetworkFilter[Network Filters: e.g. HCM HttpConnectionManager]
    NetworkFilter --> HTTPFilter[HTTP Filters: Router, Auth, RateLimit]
    HTTPFilter --> Upstream[Upstream Cluster Endpoint]
```

---

## 1. Lock-Free Design
Worker threads run without cross-thread lock contention. Configuration updates from xDS are pushed via Thread-Local Storage (TLS) pointers atomically.
