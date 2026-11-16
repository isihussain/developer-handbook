# API Protection: Distributed Token Bucket Algorithm with Redis

To enforce global rate limits without race conditions across multiple API gateway nodes, token replenishment and consumption are evaluated atomically via Redis Lua scripts.

```mermaid
sequenceDiagram
    participant Gateway as API Gateway Node
    participant Redis as Redis Cluster (Lua Script)

    Gateway->>Redis: EVALSHA token_bucket.lua (Key='rate:user_42', Rate=10, Capacity=20)
    Note over Redis: Computes newly generated tokens = (now - last_updated) * rate
    Note over Redis: current_tokens = min(capacity, current_tokens + new_tokens)
    Note over Redis: If current_tokens >= 1: current_tokens -= 1; Return 1 (ALLOWED)
    Note over Redis: Else: Return 0 (REJECTED / 429)
    Redis-->>Gateway: 1 (Allowed)
    Gateway-->>Client: HTTP 200 OK
```

---

## 1. Atomicity
The entire token generation and deduction execute inside a single Redis single-threaded Lua script, completely preventing multi-gateway race conditions.
