# Vector Embeddings & HNSW Graph Indexing

Approximate Nearest Neighbor (ANN) search and cosine distance.

## Overview
Technical cheat sheet for `vector-embeddings-hnsw-indexing` covering production deployment, configuration, and diagnostics.

## Commands and Usage
```bash
SELECT * FROM items ORDER BY embedding <=> '[0.1, 0.2]' LIMIT 10;
```
