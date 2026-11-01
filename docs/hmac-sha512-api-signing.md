# HMAC-SHA512 Webhook & API Request Signing

Tamper-proofing HTTP requests using secret cryptographic keys.

## Overview
Technical cheat sheet for `hmac-sha512-api-signing` covering production deployment, configuration, and diagnostics.

## Commands and Usage
```bash
echo -n 'payload' | openssl dgst -sha512 -hmac 'api_secret'
```
