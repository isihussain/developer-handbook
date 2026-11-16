# Kubernetes Networking: iptables ClusterIP Random Probability Balancing

In iptables mode, kube-proxy balances incoming service traffic across $N$ backend endpoints using statistical random probability rules.

```mermaid
flowchart TD
    PacketIn[Packet arrives for ClusterIP: 10.96.0.10:80] --> Rule1{"KUBE-SVC: Rule 1: statistic mode random probability 1/3?"}
    Rule1 -->|Matches 33%| DNAT1[DNAT to Pod IP 10.244.1.5]
    Rule1 -->|No match| Rule2{"Rule 2: statistic mode random probability 1/2?"}
    Rule2 -->|Matches 50% of remaining (33%)| DNAT2[DNAT to Pod IP 10.244.2.8]
    Rule2 -->|No match| DNAT3[DNAT to Pod IP 10.244.3.12 (100% of remaining 33%)]
```

---

## 1. Mathematical Guarantee
The nested probabilities $\frac{1}{N}, \frac{1}{N-1}, \dots, 1$ ensure an exact uniform $\frac{1}{N}$ probability distribution across all healthy backend pods.
