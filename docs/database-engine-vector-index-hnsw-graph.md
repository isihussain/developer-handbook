# Vector Databases: Hierarchical Navigable Small World (HNSW) Indexing

HNSW accelerates Approximate Nearest Neighbor (ANN) vector search by constructing a multi-layer geometric graph where upper layers provide long-range skips and bottom layers perform dense local refinement.

```mermaid
flowchart TD
    subgraph Layer2[Layer 2: Sparse Long-Distance Links]
        L2_Entry[Entry Point Node] --> L2_Target[Far Target Node]
    end
    subgraph Layer1[Layer 1: Medium-Density Skip Links]
        L1_N1 --> L1_N2 --> L1_N3
    end
    subgraph Layer0[Layer 0: Dense Base Graph - All Vectors Included]
        L0_N1 --> L0_N2 --> L0_N3 --> L0_KNN[Nearest Neighbors]
    end
    L2_Target -->|Greedy Search Minimum| L1_N1
    L1_N3 -->|Greedy Search Minimum| L0_N1
```

---

## 1. Algorithmic Parameters

* **`M`:** Maximum number of bidirectional connection edges per node in the graph.
* **`efConstruction`:** Size of the dynamic candidate list evaluated during index construction; trades build time for graph connectivity.
* **`efSearch`:** Size of the priority queue evaluated during query execution; balances recall accuracy against query latency.
* **Heuristic Pruning:** Replaces simple distance sorting with geometric diversity pruning to avoid clustering redundant edges in identical angular directions.
