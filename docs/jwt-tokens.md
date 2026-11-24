# JSON Web Tokens (JWT)

A JWT carries claims in a compact format. A signed JWT (JWS) protects integrity and authenticity when validated with the expected key; it does **not** hide the payload. The header and payload are base64url encoded, not encrypted. Use JWE when the claims need confidentiality.

A compact JWS has three dot-separated parts:

`base64url(header).base64url(payload).base64url(signature)`

## 1. Validate before trusting claims

Use a maintained JWT library and apply the token profile required by your application:

1. Allow only the algorithms configured for the issuer. Do not let the token's `alg` header choose an unrestricted algorithm.
2. Verify the signature with a key bound to the expected issuer.
3. Validate `iss`, `aud` when applicable, `exp`, and `nbf` when present or required by the profile.
4. Reject unexpected token types, missing required claims, and tokens intended for another service.

Decoding a JWT only reads its contents. It does not authenticate the issuer or make the claims safe to use.

## 2. HMAC signature component (demonstration only)

```bash
echo -n 'header.payload' | openssl dgst -sha256 -hmac 'development-only-secret'
```

This computes an HMAC over a string; it is not a complete JWT implementation or a validation procedure. Production secrets need sufficient entropy, secure storage, rotation, and an application-approved key-management design.

## References

- IETF, [RFC 7519: JSON Web Token](https://datatracker.ietf.org/doc/html/rfc7519).
- IETF, [RFC 8725: JSON Web Token Best Current Practices](https://datatracker.ietf.org/doc/html/rfc8725).
