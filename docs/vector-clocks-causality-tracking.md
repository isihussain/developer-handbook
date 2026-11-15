# Vector Clocks & Causal Dependency Tracking Architecture

Vector clocks determine partial ordering and detect concurrent conflicts in distributed systems without synchronized physical wall clocks.

```mermaid
flowchart TD
    A[Node A: V_A = 1, 0, 0] -->|Send message to B| B[Node B receives message]
    B --> Merge[V_B = max V_A, V_B + increment local: 1, 1, 0]
    B -->|Send message to C| C[Node C merges: 1, 1, 1]
    Concurrent[Node D concurrent write: 0, 0, 1] -.->|Concurrent Conflict Detected| Compare[Neither V_C <= V_D nor V_D <= V_C]
```

---

## 1. Causality Comparison Rule

For vector clocks $V_1$ and $V_2$:
$$V_1 \le V_2 \iff orall k.\ V_1[k] \le V_2[k]$$
$$V_1 < V_2 \iff (V_1 \le V_2) \land (\exists k.\ V_1[k] < V_2[k])$$
If neither $V_1 \le V_2$ nor $V_2 \le V_1$, the operations are **concurrent** ($V_1 \parallel V_2$) and require resolution.
