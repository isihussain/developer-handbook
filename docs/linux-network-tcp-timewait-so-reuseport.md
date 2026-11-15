# Linux Networking: TCP TIME_WAIT Mechanics & SO_REUSEPORT Architecture

The TCP `TIME_WAIT` state ($2 	imes 	ext{MSL} pprox 60	ext{s}$) prevents delayed packets from corrupting subsequent connections, while `SO_REUSEPORT` enables multi-core socket load balancing.

```mermaid
flowchart TD
    subgraph TIME_WAIT[TIME_WAIT Lifecycle: 2 * MSL]
        Close[Active Close: Sends FIN] --> LastACK[Receives ACK of FIN]
        LastACK --> TW[Enters TIME_WAIT: 60 seconds]
        TW -->|Guarantees peer receives final ACK| Closed[Closed State]
    end
    subgraph Reuseport[SO_REUSEPORT Multi-Core Socket Scaling]
        ListenSock[Port 443 Shared by 4 Worker Threads] --> Worker0[Worker Core 0: Independent accept() queue]
        ListenSock --> Worker1[Worker Core 1: Independent accept() queue]
        ListenSock --> Worker2[Worker Core 2: Independent accept() queue]
        ListenSock --> Worker3[Worker Core 3: Independent accept() queue]
    end
```

---

## 1. Kernel Optimization Rules

* **`SO_REUSEPORT`:** Allows multiple processes/threads to bind the same port; incoming SYNs are load-balanced via 4-tuple hashing directly to per-socket listen queues, eliminating accept mutex contention.
* **`tcp_tw_reuse`:** Safely reallocates an outgoing `TIME_WAIT` socket for new client connections if timestamp validation (`TCP Timestamps RFC 7323`) proves packet freshness.
* **Avoid `tcp_tw_recycle`:** Removed from modern Linux kernels due to NAT incompatibilities that randomly dropped valid connections sharing public IP addresses.
