# Web Standards: Voluntary Application Server Identification (VAPID)

RFC 8292 VAPID allows application servers to authenticate directly with push services (FCM, Apple Push, Mozilla Push) without vendor-specific accounts.

```mermaid
sequenceDiagram
    participant App as Application Server (VAPID Private Key)
    participant PushService as Browser Push Service (Apple/Google)
    participant Browser as End-User Browser

    Note over App: Signs JWT with VAPID Private Key (aud, exp, sub)
    App->>PushService: POST /push-endpoint (Authorization: vapid t=...,k=...)
    PushService->>PushService: Verifies JWT signature using VAPID Public Key
    PushService-->>App: 201 Created
    PushService->>Browser: Delivers Encrypted Web Push Notification!
```

---

## 1. Encryption Invariant
Push payloads are encrypted end-to-end using AES-128-GCM with ECDH keys generated in the browser; the intermediate push service cannot inspect payload contents.
