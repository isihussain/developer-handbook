# Web Security: Content Security Policy Level 3 Strict Nonce Architecture

Legacy domain allowlists in CSP are easily bypassed via JSONP endpoints and open redirects. Strict Nonce CSP defeats Cross-Site Scripting (XSS) by verifying dynamic cryptographic nonces.

```mermaid
flowchart TD
    Req[Incoming HTTP Request] --> GenNonce[Generate Cryptographically Random Nonce: 'rAnd0m123']
    GenNonce --> Header["Header: Content-Security-Policy: script-src 'nonce-rAnd0m123' 'strict-dynamic'"]
    GenNonce --> HTMLBody["HTML: <script nonce='rAnd0m123' src='/app.js'></script>"]
    Browser[Browser Parser] --> EvalScript{Script has matching nonce?}
    EvalScript -->|Yes| Execute[Execute JavaScript]
    EvalScript -->|No: Injected XSS Payload| Block[BLOCK SCRIPT EXECUTION!]
```

---

## 1. `'strict-dynamic'` Directive
Enables scripts that carry a valid nonce to dynamically load downstream dependencies without explicitly declaring every vendor URL in the policy header.
