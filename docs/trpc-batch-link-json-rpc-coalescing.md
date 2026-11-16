# Fullstack Architecture: tRPC httpBatchLink Request Coalescing

tRPC delivers end-to-end TypeScript type contracts without code generation, grouping multiple client query calls into a single batched HTTP request.

```mermaid
flowchart LR
    ComponentA[Component 1: trpc.user.useQuery()] --> BatchLink[tRPC httpBatchLink Queue]
    ComponentB[Component 2: trpc.settings.useQuery()] --> BatchLink
    ComponentC[Component 3: trpc.notifications.useQuery()] --> BatchLink
    BatchLink -->|Single HTTP Request| Endpoint["POST /api/trpc/user,settings,notifications?batch=1"]
    Endpoint --> Server[Server Router: Parallel internal resolution]
    Server --> Response["HTTP 200 OK: [ {result: user}, {result: settings}, {result: notifications} ]"]
```

---

## 1. Network Optimization
Reduces HTTP round-trip overhead and connection pool exhaustion on dashboard views that load numerous widget data sources concurrently.
