# OLAP Query Engines: DuckDB Morsel-Driven Parallelism

DuckDB parallelizes analytical SQL queries using fine-grained work stealing over 100,000-row chunks called **Morsels**.

```mermaid
flowchart TD
    Pipeline[Analytical Query Pipeline] --> Scheduler[Morsel Scheduler]
    Scheduler --> Worker1[Worker Thread 1: Claims Morsel 0-100K]
    Scheduler --> Worker2[Worker Thread 2: Claims Morsel 100K-200K]
    Worker1 --> Finish[Worker 1 finishes early!]
    Finish --> Steal[Steals next available Morsel: Zero Core Idle Time!]
```

---

## 1. NUMA Locality
Threads schedule morsels residing on their local NUMA node first before stealing from remote socket memory.
