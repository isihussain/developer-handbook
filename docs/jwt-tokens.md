# JSON Web Tokens (JWT)

Header, Payload, Signature anatomy, HMAC-SHA256 vs RS256, and expiration handling.

## Overview
Technical cheat sheet for `jwt-tokens` covering production deployment, configuration, and diagnostics.

## Commands and Usage
```bash
echo -n 'header.payload' | openssl dgst -sha256 -hmac 'secret'
```
