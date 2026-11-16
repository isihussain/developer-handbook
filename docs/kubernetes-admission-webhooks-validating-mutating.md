# Kubernetes Security: Dynamic Admission Controller Webhook Pipeline

Admission webhooks intercept requests to the `kube-apiserver` before objects are persisted to etcd.

```mermaid
sequenceDiagram
    participant Client as kubectl / CI/CD
    participant API as kube-apiserver
    participant Mutating as Mutating Webhook (e.g. Sidecar Injector)
    participant Schema as Object Schema Validation
    participant Validating as Validating Webhook (e.g. OPA Gatekeeper)
    participant etcd as etcd Cluster

    Client->>API: Create Pod YAML
    API->>Mutating: POST AdmissionReview
    Mutating-->>API: JSONPatch (Inject Sidecar, Resources, SecurityContext)
    API->>Schema: Validate OpenAPI Schema
    API->>Validating: POST AdmissionReview
    Validating-->>API: Allowed: true / false
    API->>etcd: Persist to Storage!
```

---

## 1. Execution Order Invariant
Mutating webhooks ALWAYS run before Validating webhooks so that policies can audit the final, fully-mutated state of the object.
