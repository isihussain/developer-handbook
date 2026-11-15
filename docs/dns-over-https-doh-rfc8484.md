# Network Protocols: DNS-over-HTTPS (DoH RFC 8484)

DNS-over-HTTPS encrypts DNS queries inside HTTP/2 and HTTP/3 streams over port 443, mitigating ISP eavesdropping, cache poisoning, and DNS hijacking.

```mermaid
sequenceDiagram
    participant Client
    participant Resolver as DoH Resolver (1.1.1.1 / 8.8.8.8)
    Note over Client,Resolver: TLS 1.3 Handshake on Port 443
    Client->>Resolver: POST /dns-query (Content-Type: application/dns-message)
    Note over Resolver: Parses wire-format DNS request
    Resolver->>Client: 200 OK (Content-Type: application/dns-message)
```

---

## 1. Protocol Specifics

* **Wire Format Body:** Payloads contain standard binary DNS messages (RFC 1035) transmitted without translation.
* **HTTP Methods:** Supports `POST` (binary body) and `GET` (base64url-encoded `?dns=` query string parameter), leveraging standard HTTP caching headers (`cache-control: max-age`).
