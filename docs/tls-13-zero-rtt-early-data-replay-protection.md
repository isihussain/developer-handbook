# Transport Security: TLS 1.3 0-RTT Early Data and Replay Attacks

A client with a previously established PSK can send application data in its first handshake flight. This saves a round trip, but TLS 1.3 does not provide inherent replay protection for 0-RTT data across connections or server instances.

```mermaid
sequenceDiagram
    participant Client
    participant Server
    Client->>Server: ClientHello + PSK + early data
    Note over Server: Data may be replayed; apply application policy
    Server-->>Client: ServerHello + EncryptedExtensions + Finished
    Server-->>Client: Handshake completes
```

## 1. Application safety

A client should send early data only when the application deems it safe to replay. “Idempotent” is a useful starting point, but repeated requests can still leak information, consume resources, or trigger side effects outside the operation itself. Payment, account changes, and other non-repeatable actions should wait until the handshake completes unless the application has explicit replay-safe semantics.

Servers that accept early data can track accepted tickets or otherwise coordinate replay state. A single-use ticket cache helps only when the relevant server instances share that state; a per-node cache does not prevent an attacker from replaying the same ClientHello to another node. Applications should also handle the possibility that a client retries a request after early data was rejected.

## References

- IETF, [RFC 8446, Section 8: 0-RTT and Anti-Replay](https://datatracker.ietf.org/doc/html/rfc8446#section-8).
