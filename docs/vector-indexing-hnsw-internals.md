# HNSW: Hierarchical Navigable Small World Vector Indexing

Hierarchical Navigable Small World (HNSW) organizes high-dimensional embeddings into multi-layer proximity graphs with logarithmic search complexity, inspired by skip-lists.

```mermaid
flowchart TD
    subgraph Layer2[Layer 2: Sparse Highway]
        L2_1[Node A] --- L2_2[Node D]
    end
    subgraph Layer1[Layer 1: Intermediate Density]
        L1_1[Node A] --- L1_2[Node B] --- L1_3[Node D]
    end
    subgraph Layer0[Layer 0: Dense Proximity Graph - All Vectors]
        L0_1[Node A] --- L0_2[Node B] --- L0_3[Node C] --- L0_4[Node D] --- L0_5[Node E]
    end
    Layer2 -.-> Layer1 -.-> Layer0
```

---

## 1. Parameters & Tradeoffs

1. **$M$ (Max Edges per Node):** Controls memory footprint and connectivity graph degree. Typical range: 16 to 64. Higher values boost recall on clustering datasets.
2. **$efSearch$ (Search Exploration Factor):** Size of the dynamic candidate priority queue evaluated at query time. Tradeoff: higher recall vs increased query latency.
3. **$efConstruction$:** Search horizon size during node insertion. Governs index build speed and quality.
