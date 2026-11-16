# Production Reliability: Pod Termination Lifecycle and preStop Hooks

Terminating pods without dropping in-flight client requests requires orchestrating endpoint removal concurrently with application graceful shutdown.

```mermaid
sequenceDiagram
    participant API as kube-apiserver
    participant EP as EndpointSlice Controller
    participant Kubelet as Node Kubelet
    participant Pod as Application Container

    API->>EP: Mark Pod Terminating (Remove from Service Endpoints)
    API->>Kubelet: Initiate Pod Termination
    Kubelet->>Pod: Execute preStop hook (sleep 10s: Allows DNS/kube-proxy propagation)
    Note over Pod: Finishes in-flight HTTP transactions
    Kubelet->>Pod: Send SIGTERM
    Note over Pod: Application closes open database pools & socket listeners
    Kubelet->>Pod: If still running after terminationGracePeriodSeconds -> Send SIGKILL!
```

---

## 1. Race Condition Fix
Without a `preStop: exec: command: ["sleep", "10"]`, the pod receives SIGTERM before iptables/IPVS rules remove its IP from upstream load balancers, causing 502 Bad Gateway errors.
