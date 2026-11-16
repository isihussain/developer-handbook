# Network Programming: TCP Keepalive Probe Architecture

When network routers silently drop NAT mapping states or endpoints crash, TCP connections hang indefinitely unless Keepalive probes are configured.

```mermaid
flowchart TD
    IdleConnection[TCP Connection Idle for tcp_keepalive_time: 7200s (2 hours)] --> TriggerProbe[Send 1-byte ACK probe with sequence number SEG.UNA - 1]
    TriggerProbe --> CheckResponse{Peer responds with ACK within tcp_keepalive_intvl (75s)?}
    CheckResponse -->|Yes| ResetTimer[Reset Idle Timer: Connection Alive!]
    CheckResponse -->|No| RetryProbe[Send Probe Retry: up to tcp_keepalive_probes (9 times)]
    RetryProbe -->|Exceeded Retries| Terminate[Close Socket with -ETIMEDOUT!]
```

---

## 1. Cloud-Native Tuning
Default 2-hour keepalive is disastrous for cloud load balancers that drop idle conntrack states after 350 seconds.
- `tcp_keepalive_time = 300`
- `tcp_keepalive_intvl = 30`
- `tcp_keepalive_probes = 3`
