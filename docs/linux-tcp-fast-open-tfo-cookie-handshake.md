# Web Latency: RFC 7413 TCP Fast Open (TFO) Architecture

TCP Fast Open enables data payload transmission directly inside the initial `SYN` packet, saving one full round-trip time (1 RTT) during HTTP connection setup.

```mermaid
sequenceDiagram
    participant Client
    participant Server

    Note over Client: Initial Handshake: Client requests TFO Cookie
    Client->>Server: SYN (with TFO Cookie Request option)
    Server-->>Client: SYN-ACK (with encrypted 16-byte TFO Cookie)
    Client->>Server: ACK
    Note over Client,Server: Subsequent Connection: 0-RTT TCP!
    Client->>Server: SYN + TFO Cookie + HTTP GET /index.html in same packet!
    Server-->>Client: SYN-ACK + HTTP 200 OK payload!
```

---

## 1. sysctl Activation
```bash
echo 3 > /proc/sys/net/ipv4/tcp_fastopen # Enables client (1) and server (2) TFO
```
