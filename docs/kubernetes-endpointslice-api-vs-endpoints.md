# Kubernetes Control Plane: EndpointSlice API Architecture

The legacy `Endpoints` resource stored all pod IPs for a Service in a single object. For a service with 5,000 pods, a single pod update triggered full serialization of the entire 1.5MB object.

```mermaid
flowchart TD
    Service[Service: 'payment-svc' with 5000 Pods] --> Slices[EndpointSlice Controller]
    Slices --> Slice1[EndpointSlice 1: Max 100 Pod IPs]
    Slices --> Slice2[EndpointSlice 2: Max 100 Pod IPs]
    Slices --> SliceN[EndpointSlice N: Max 100 Pod IPs]
    PodUpdate[Pod IP Changed in Slice 2] --> SelectiveUpdate[Update ONLY Slice 2: 98% Network Bandwidth Reduction!]
```

---

## 1. Architectural Limit
Each `EndpointSlice` holds at most 100 endpoints by default, eliminating etcd write amplification and kube-proxy serialization bottlenecks.
