# Kernel TLS (kTLS): Zero-Copy Splicing & Inline NIC Offload

Kernel TLS (kTLS) moves symmetric cryptographic framing (AES-GCM / ChaCha20-Poly1305) into the Linux kernel TCP stack, enabling zero-copy `sendfile()` over encrypted sessions.

```mermaid
flowchart LR
    subgraph SoftwareTLS[Standard OpenSSL: Double Copy]
        App1[Application Buffer] --> Crypto1[Userspace Crypto Engine]
        Crypto1 --> Socket1[Kernel TCP Socket]
    end
    subgraph kTLS[Kernel TLS: Zero-Copy NVMe to Network]
        Disk[Disk Page Cache] -->|sys_sendfile| kTLSEngine[Kernel kTLS Module]
        kTLSEngine -->|Encrypts in place via AES-NI / NIC Offload| NIC[Network Interface Card]
    end
```

---

## 1. Socket Configuration

* **Socket Option:** Configured via `setsockopt(fd, SOL_TLS, TLS_TX, &crypto_info, sizeof(crypto_info))` after user-space handshake completion.
* **SmartNIC Offload:** Compatible NICs (Mellanox ConnectX-6+) perform hardware encryption directly on the wire without consuming CPU cycles.
