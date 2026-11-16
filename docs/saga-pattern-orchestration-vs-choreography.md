# Microservices: Saga Pattern Orchestration vs Choreography

A Saga is a sequence of local transactions where each step updates a service database and publishes an event; if a step fails, compensating transactions undo preceding changes.

```mermaid
flowchart TD
    subgraph Orchestration
        O[Saga Orchestrator] --> S1[Order Service]
        S1 --> O
        O --> S2[Payment Service: FAILS]
        S2 -->|Fail| O
        O -->|Compensate| Undo1[Order Service: Cancel Order]
    end
```

---

## 1. Comparison Matrix

| Dimension | Orchestration | Choreography |
| :--- | :--- | :--- |
| **Control** | Centralized orchestrator state machine | Decentralized event pub/sub |
| **Coupling** | Services know only orchestrator | Services must know downstream events |
| **Debugging** | Clear audit trace in orchestrator | Distributed tracing required across queues |
