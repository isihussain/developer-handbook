# Progressive Web Apps: Service Worker Cache Storage Strategies

Stale-While-Revalidate responds immediately from Cache Storage while fetching fresh assets in the background to update the cache.

```mermaid
sequenceDiagram
    participant Page as Web Browser Page
    participant SW as Service Worker
    participant Cache as Cache Storage (IndexedDB)
    participant Network as CDN / Origin Server

    Page->>SW: fetch('/api/user')
    SW->>Cache: match('/api/user')
    Cache-->>Page: Return Cached Response (Instant: 2ms!)
    SW->>Network: fetch('/api/user') (Background)
    Network-->>SW: Fresh Response 200 OK
    SW->>Cache: put('/api/user', freshResponse)
```

---

## 1. Offline Reliability
Guarantees fast load times on flaky cellular networks while keeping client data eventually consistent.
