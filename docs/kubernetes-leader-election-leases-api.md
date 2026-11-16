# High Availability: Kubernetes Leader Election with Coordination Leases

Kubernetes controllers (kube-controller-manager, cert-manager, ingress controllers) coordinate active-passive failover using the `coordination.k8s.io/v1` Leases API.

```mermaid
sequenceDiagram
    participant Pod1 as Controller Pod 1 (Active Leader)
    participant API as Kubernetes API Server (Lease Object)
    participant Pod2 as Controller Pod 2 (Passive Standby)

    Note over Pod1: Renews Lease: PATCH /leases/ingress-controller (acquireTime, renewTime)
    Pod1->>API: Heartbeat renew every 2 seconds
    Note over Pod1: Pod 1 crashes!
    Note over Pod2: Observes LeaseDurationSeconds (15s) expired!
    Pod2->>API: Atomic CAS: Acquire Lease holderIdentity='pod-2'
    Note over Pod2: Pod 2 becomes Active Leader!
```

---

## 1. Split-Brain Prevention
Uses resourceVersion optimistic concurrency control on the Lease object. If two pods attempt to renew simultaneously, the slower CAS operation fails with a 409 Conflict.
