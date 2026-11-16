# Network Privacy: DNS-over-HTTPS (DoH) vs DNS-over-TLS (DoT)

Traditional plaintext DNS (UDP port 53) exposes user domain lookups to ISP eavesdropping and on-path spoofing. Encrypted DNS wraps queries in TLS.

```mermaid
flowchart LR
    Client -->|DoT: Dedicated Port 853| DoT_Server[DNS-over-TLS: Easily Blocked by Firewalls]
    Client -->|DoH: Port 443 (HTTP/2 / HTTP/3)| DoH_Server[DNS-over-HTTPS: Indistinguishable from Web Traffic!]
```

---

## 1. Comparison Matrix

| Dimension | DoT (RFC 7858) | DoH (RFC 8484) |
| :--- | :--- | :--- |
| **Port** | TCP 853 | TCP 443 (Standard HTTPS) |
| **Transport** | Raw TLS stream | HTTP/2 or HTTP/3 binary wireformat |
| **Censorship Evasion** | Low (Trivial for firewalls to block port 853) | High (Blends with all web traffic) |
