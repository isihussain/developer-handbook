# Distributed Caching: Consistent Hashing with Virtual Nodes

Consistent hashing minimizes key redistribution when cache nodes join or leave, mapped across a continuous circular hash ring ($[0, 2^{32}-1]$).

```mermaid
flowchart TD
    Ring((Circular Hash Ring: 0 to 2^32-1))
    Key1[Key 'user_102'] --> Ring
    Key2[Key 'session_99'] --> Ring
    Ring --> NodeA_v1[Node A - Vnode 1]
    Ring --> NodeB_v1[Node B - Vnode 1]
    Ring --> NodeA_v2[Node A - Vnode 2]
    Ring --> NodeB_v2[Node B - Vnode 2]
```

---

## 1. Google Bounded-Load Variant
Standard consistent hashing can result in hotspot nodes. The Mirrokni et al. bounded-load algorithm enforces:
$$\text{Load}(Node) \le (1 + \epsilon) \cdot \text{AverageLoad}$$
If a target node is above the bound, the key walks clockwise to the next available replica.
