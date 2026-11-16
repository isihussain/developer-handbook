# Supply Chain Security: Subresource Integrity (SRI) Validation

Subresource Integrity guarantees that third-party CDN scripts or stylesheets have not been maliciously tampered with by verifying a cryptographic base64 hash.

```mermaid
sequenceDiagram
    participant Browser
    participant CDN as Third-Party CDN (Compromised)

    Note over Browser: <script src="https://cdn.example.com/lib.js" integrity="sha384-xyz..." crossorigin="anonymous">
    Browser->>CDN: Fetch lib.js
    CDN-->>Browser: Injected with Keylogger Payload
    Note over Browser: Computes SHA-384 of received payload
    Note over Browser: Hash Mismatch Detected!
    Note over Browser: Aborts execution immediately with NetworkError!
```

---

## 1. CORS Requirement
SRI requires `crossorigin="anonymous"` so that the browser is permitted to read and hash the foreign script bytes before execution.
