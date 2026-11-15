# Chaitin-Briggs Graph Coloring Register Allocation

Register allocation models virtual register live ranges as an undirected interference graph, mapping variables to $K$ physical registers by reducing the problem to NP-complete $K$-graph coloring.

```mermaid
flowchart TD
    Build[1. Build: Construct Interference Graph from Live Ranges] --> Simplify[2. Simplify: Remove nodes with degree < K]
    Simplify -->|No nodes with degree < K| Spill[3. Spill: Select lowest cost variable to spill to stack]
    Simplify --> Select[4. Select: Re-insert nodes and assign physical color]
    Spill --> Recompute[Recompute live intervals & rebuild graph]
    Recompute --> Build
    Select --> Complete[Register Allocation Complete]
```

---

## 1. Kempe's Heuristic

If a node $v$ has fewer than $K$ neighbors ($\deg(v) < K$), removing $v$ from the graph leaves a subgraph that is $K$-colorable if and only if the original graph is $K$-colorable.
