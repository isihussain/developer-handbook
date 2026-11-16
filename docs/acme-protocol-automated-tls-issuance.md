# Security Automation: ACME Protocol (RFC 8555) Architecture

The Automated Certificate Management Environment (ACME) protocol powers Let's Encrypt, automating domain validation and certificate renewal without human intervention.

```mermaid
sequenceDiagram
    participant Client as ACME Client (Certbot / Caddy)
    participant Server as ACME CA Server (Let's Encrypt)
    participant DNS as Authoritative DNS / HTTP Server

    Client->>Server: NewOrder (example.com)
    Server-->>Client: Challenge: HTTP-01 or DNS-01 Token
    Client->>DNS: Provision Token at /.well-known/acme-challenge/
    Client->>Server: Challenge Ready
    Server->>DNS: Multi-Perspective Validation Probe
    Server-->>Client: Validation Valid!
    Client->>Server: Submit CSR
    Server-->>Client: Issue Signed X.509 Certificate
```

---

## 1. Multi-Perspective Validation
ACME servers validate challenges from multiple vantage points globally to defeat localized BGP hijacking and DNS cache poisoning.
