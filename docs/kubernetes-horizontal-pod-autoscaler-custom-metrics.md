# Kubernetes Autoscaling: Horizontal Pod Autoscaler (HPA) Custom Metrics

HPA scales pod replicas dynamically based on CPU/Memory or custom business metrics (e.g. SQS queue length, HTTP requests/second).

```mermaid
flowchart LR
    Prometheus[Prometheus: metrics 'http_requests_per_second'] --> Adapter[Prometheus Adapter / KEDA]
    Adapter --> CustomAPIServer[Custom Metrics API: custom.metrics.k8s.io]
    CustomAPIServer --> HPA[HPA Controller: Queries metric every 15s]
    HPA --> Calc["Desired Replicas = ceil(CurrentReplicas * (CurrentMetric / TargetMetric))"]
    Calc --> Scale[Scale Deployment Replica Count]
```

---

## 1. Stabilization Window
The `behavior.scaleDown.stabilizationWindowSeconds` (default 300s) prevents flapping (rapid scaling up and down) during bursty traffic patterns.
