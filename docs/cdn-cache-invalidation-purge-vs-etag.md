# Web Infrastructure: CDN Cache Invalidation & Stale-While-Revalidate

Managing global CDN edge cache consistency requires balancing instant API purge broadcasts against conditional HTTP validation.

```mermaid
flowchart TD
    NewDeploy[New Static Release Deployed] --> InvalidationChoice{Cache Invalidation Strategy}
    InvalidationChoice -->|Content Hash in URL| AssetHashing["/static/app.8f2a1b.js: Cache-Control: max-age=31536000, immutable: 100% Cache Hits!"]
    InvalidationChoice -->|Surrogate-Key / Tag Purge| FastPurge["Fastly / Cloudflare API: Purge Surrogate-Key 'blog-102': Global invalidation in 150ms!"]
    InvalidationChoice -->|Conditional ETag Revalidation| ETagCheck["If-None-Match: Returns HTTP 304 Not Modified (1 RTT penalty)"]
```

---

## 1. Best Practice
Use immutable content-hashed URLs for static frontend assets (`bundle.[hash].js`) and Surrogate-Keys (Cache-Tags) for dynamic API data.
