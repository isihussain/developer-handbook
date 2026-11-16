# API Optimization: GraphQL N+1 Query Problem & DataLoader Solution

Naive GraphQL resolvers execute individual database queries for each item in a parent collection, resulting in $N + 1$ database round-trips.

```mermaid
sequenceDiagram
    participant Resolver as GraphQL Execution Engine
    participant Loader as DataLoader Batching Cache
    participant DB as SQL Database

    Note over Resolver: Resolving authors for 50 books
    Resolver->>Loader: loader.load(author_id_1)
    Resolver->>Loader: loader.load(author_id_2) ... (50 calls)
    Note over Loader: Coalesces IDs within same event loop tick!
    Loader->>DB: SELECT * FROM authors WHERE id IN (1, 2, ..., 50) (Single Query!)
    DB-->>Loader: Array of 50 Authors
    Loader-->>Resolver: Dispatches individual author objects to resolvers
```

---

## 1. Event Loop Coalescing
DataLoader uses Node.js `process.nextTick()` or Python `asyncio` ticks to collect all keys requested in the current tick before firing the batch database fetch.
