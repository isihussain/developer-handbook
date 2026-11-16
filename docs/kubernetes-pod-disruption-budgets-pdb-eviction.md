# Kubernetes Operations: PodDisruptionBudget and Eviction API

PDBs guarantee application availability during voluntary disruptions (e.g., node drains for AMI updates or cluster upgrades).

```mermaid
sequenceDiagram
    participant Admin as Cluster Admin / Autoscaler
    participant API as Kubernetes Eviction API
    participant PDB as PodDisruptionBudget Controller
    participant Kubelet as Node Kubelet

    Admin->>API: POST /api/v1/namespaces/prod/pods/app-1/eviction
    API->>PDB: Check currentHealthyPods - 1 >= minAvailable
    PDB-->>API: PDB satisfied! (Allowed)
    API->>Kubelet: Send SIGTERM to Pod app-1
    Note over Admin,API: If condition fails: API returns 429 Too Many Requests!
```

---

## 1. Specification Rules
- `minAvailable: 80%` or `maxUnavailable: 1`
- Only applies to voluntary disruptions (`kubectl drain`), not hardware crashes or node kernel panics.
