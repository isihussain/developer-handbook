# Distributed Storage: Vector Clocks & Dotted Version Vectors (DVV)

Vector Clocks and Dotted Version Vectors establish partial causal ordering across decentralized distributed databases (e.g., Dynamo, Riak), enabling accurate detection of concurrent conflicting updates.

```mermaid
flowchart TD
    ClientWrite[Client Update on Node A] --> Incr[Node A increments its clock entry: V[A] = V[A] + 1]
    Incr --> Msg[Message carries Vector V: A:2, B:1, C:0]
    Msg --> NodeB[Node B Receives Message]
    NodeB --> Merge["Merge: V_B[k] = max(V_B[k], V_msg[k]) for all nodes k"]
```

---

## 1. Causal Comparison Rules

For two vector clock states $V_1$ and $V_2$:
* **$V_1 \le V_2$:** Holds if and only if $orall k, V_1[k] \le V_2[k]$.
* **$V_1 < V_2$ ($V_1$ causally preceded $V_2$):** Holds if $V_1 \le V_2$ and $\exists k$ such that $V_1[k] < V_2[k]$.
* **Concurrent ($V_1 \parallel V_2$):** If neither $V_1 \le V_2$ nor $V_2 \le V_1$ holds, the updates occurred concurrently; storage engines flag the records as conflicted and present siblings to the application.
