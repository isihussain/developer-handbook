# Vector Clocks & Distributed Causal Ordering

Tracks causal history in distributed key-value stores (Amazon Dynamo, Riak) to detect concurrent write conflicts.

---

## 1. Partial Order Comparison

Given vector timestamps $V_A$ and $V_B$:
- $V_A \le V_B \iff orall k, V_A[k] \le V_B[k]$.
- $V_A < V_B \iff V_A \le V_B 	ext{ and } V_A 
e V_B$ ($A$ causally preceded $B$).
- If neither $V_A \le V_B$ nor $V_B \le V_A$, events $A$ and $B$ are **concurrent** (conflict requires merge or sibling resolution).
