# IVF-PQ: Inverted File with Product Quantization

Product Quantization compresses vector embeddings into compact byte codes, enabling billion-scale similarity search within RAM limits.

```mermaid
flowchart LR
    subgraph VectorSplit[Original 128-dim Float32: 512 Bytes]
        Sub1[Sub-vector 1: 16-dim]
        Sub2[Sub-vector 2: 16-dim]
        Sub8[Sub-vector 8: 16-dim]
    end
    Sub1 -->|Cluster centroids| Code1["Code 1 (1 byte: 0-255)"]
    Sub2 -->|Cluster centroids| Code2["Code 2 (1 byte: 0-255)"]
    Sub8 -->|Cluster centroids| Code8["Code 8 (1 byte: 0-255)"]
    Code1 & Code2 & Code8 --> Compressed["Compressed Vector: 8 Bytes (64x reduction)"]
```

---

## 1. Two-Level Indexing Pipeline

1. **Inverted File (IVF):** The vector space is partitioned into $k$ Voronoi cells via $k$-means. At query time, only the closest $n_{probe}$ inverted lists are evaluated.
2. **Asymmetric Distance Computation (ADC):** Computes distances between the uncompressed query vector and compressed dataset byte codes using pre-calculated lookup tables (LUT), avoiding full vector decompression.
