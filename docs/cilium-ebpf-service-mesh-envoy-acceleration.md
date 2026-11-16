# Service Mesh Optimization: Cilium eBPF Socket Layer Acceleration

Traditional service meshes loop packets through TCP/IP stacks multiple times to reach local sidecar proxies. Cilium accelerates socket traffic via sockmap eBPF programs.

```mermaid
flowchart LR
    AppSocket[Application Socket: write()] --> SockMap{Cilium sockmap BPF Program}
    SockMap -->|Direct Socket Memory Injection: Bypasses TCP/IP!| EnvoySocket[Envoy Proxy Ingress Socket]
    EnvoySocket --> L7_Policy[L7 WAF & Auth Evaluation]
    L7_Policy --> SockMapOut{Cilium sockmap}
    SockMapOut -->|Direct Socket Injection| TargetSocket[Target Pod Socket]
```

---

## 1. Socket Layer Bypass
The `bpf_msg_redirect_hash()` helper copies payload directly between socket buffers in kernel space, eliminating TCP state machine, checksumming, and routing table evaluation.
