# Consistent Hashing with Bounded Loads

Consistent hashing with bounded loads (Mirrokni et al., Google Research) eliminates server hot-spots in distributed caching.

```mermaid
graph TD
    Key[Incoming Key H(k)] --> Ring[Hash Ring Position]
    Ring --> Primary[Primary Server S1]
    Primary --> Capacity{Load > (1 + epsilon) * Average?}
    Capacity -->|No| Accept[Assign to S1]
    Capacity -->|Yes| Next[Walk clockwise to S2]
```

---

## 1. Load Cap Formula

Let $L_{	ext{avg}} = rac{N}{M}$ be average items per server. No server is permitted to exceed:
$$	ext{Max Capacity} = \lfloor (1 + \epsilon) L_{	ext{avg}} floor + 1$$
If the primary mapped server is at capacity, the request cascades clockwise along the ring to the next available server.
