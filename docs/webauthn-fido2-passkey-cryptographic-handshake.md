# Passwordless Security: WebAuthn / FIDO2 Passkey Cryptography

WebAuthn replaces passwords with asymmetric cryptographic keypairs generated inside hardware security keys or device secure enclaves (TouchID, FaceID).

```mermaid
sequenceDiagram
    participant Browser
    participant Enclave as Hardware Authenticator (TouchID / YubiKey)
    participant Server as Relying Party (RP)

    Server->>Browser: Challenge + RP ID ('auth.example.com')
    Browser->>Enclave: Prompt User Biometrics + Verify RP ID
    Note over Enclave: Signs Challenge + ClientDataHash with Private Key
    Enclave-->>Browser: AuthenticatorData + Signature
    Browser->>Server: Submit Assertion
    Note over Server: Verifies signature against stored Public Key!
```

---

## 1. Phishing Immunity
The client platform validates that the origin matches the RP ID. Even if a user visits a convincing phishing site (`evil-login.com`), the authenticator will sign for `evil-login.com`, which the real server will reject immediately.
