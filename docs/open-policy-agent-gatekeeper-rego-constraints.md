# Policy Enforcement: Open Policy Agent (OPA) Gatekeeper

Gatekeeper enforces declarative security policies (preventing privileged containers, mandating resource limits) via declarative Rego rules.

```mermaid
flowchart LR
    KubeAPIServer[kube-apiserver: Validating Webhook] --> Gatekeeper[Gatekeeper Pod: In-memory OPA Engine]
    Gatekeeper --> Constraint[ConstraintTemplate: Parameterized Rego Logic]
    Constraint --> Eval{Violates security rules?}
    Eval -->|Yes| Deny[Deny Admission: 403 Forbidden with explanation]
    Eval -->|No| Allow[Allow Deployment]
```

---

## 1. Audit Mode
Gatekeeper continuously audits running resources against active constraints, reporting non-compliant historical workloads without disrupting active traffic.
