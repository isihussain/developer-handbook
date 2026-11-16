# Fault Tolerance: Circuit Breaker Pattern State Machine

Circuit breakers prevent cascading failures across microservices by cutting off traffic to failing downstream dependencies.

```mermaid
stateDiagram-v2
    [*] --> Closed: Normal Operation
    Closed --> Open: Failure Rate > 50% within Rolling Window
    Open --> HalfOpen: Sleep Window (e.g. 10s) expires
    HalfOpen --> Closed: Test Probes succeed (Dependency Recovered!)
    HalfOpen --> Open: Test Probe fails (Dependency Still Down)
```

---

## 1. Failure Invariant
In the **Open** state, calls fail fast immediately without waiting for timeouts or tying up thread pool execution slots.
