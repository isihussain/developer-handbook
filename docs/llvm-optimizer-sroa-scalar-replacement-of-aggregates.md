# Compiler Optimization: LLVM SROA (Scalar Replacement of Aggregates)

LLVM SROA dismantles aggregate variables (C `struct`, C++ classes) allocated in memory into discrete scalar variables, enabling `mem2reg` to promote stack memory loads into SSA virtual registers.

```mermaid
flowchart TD
    subgraph AggregateStack[Original Code: Struct on Stack]
        Alloca["%point = alloca { i32, i32 }"]
        StoreX["store i32 10, %point.x"]
        StoreY["store i32 20, %point.y"]
        LoadX["%val = load i32, %point.x"]
    end
    Alloca --> SROA[LLVM SROA Optimization Pass]
    subgraph PromotedSSA[After SROA + mem2reg: Zero Memory Access]
        SSA_X["%point.x = 10"]
        SSA_Y["%point.y = 20"]
        Use["%val = %point.x (Direct Register Propagation)"]
    end
    SROA --> PromotedSSA
```

---

## 1. Optimization Pipeline

1. **Partition Analysis:** Inspects all `GetElementPtr` (GEP) instructions referencing an `alloca`, determining byte slices accessed by the program.
2. **Slicing:** Breaks monolithic allocations into independent scalar `alloca` instances for each non-overlapping field.
3. **Register Promotion (`mem2reg`):** Promotes newly generated independent scalar allocas directly into SSA values (`%1`, `%2`), converting memory traffic into register operations.
