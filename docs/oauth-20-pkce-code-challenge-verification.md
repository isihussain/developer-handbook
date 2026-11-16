# Identity & Access: OAuth 2.0 Authorization Code Flow with PKCE

Proof Key for Code Exchange (PKCE, RFC 7636) prevents authorization code interception attacks on public clients (Single-Page Apps, mobile apps) that cannot protect client secrets.

```mermaid
sequenceDiagram
    participant SPA as Single Page App (Browser)
    participant Auth as Authorization Server
    participant API as Resource Server

    Note over SPA: Generates random code_verifier (43-128 chars)
    Note over SPA: code_challenge = Base64URL(SHA256(code_verifier))
    SPA->>Auth: /authorize?response_type=code&code_challenge=...&code_challenge_method=S256
    Auth-->>SPA: Redirect with authorization_code
    SPA->>Auth: /token?code=...&code_verifier=...
    Note over Auth: Verifies SHA256(code_verifier) == code_challenge!
    Auth-->>SPA: Access Token + Refresh Token
```

---

## 1. Protection Guarantee
Even if a malicious native app intercepts the authorization code via custom URI schemes, it cannot exchange it for tokens because it lacks the original `code_verifier`.
