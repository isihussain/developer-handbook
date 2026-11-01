# Time-Based One-Time Password (TOTP RFC 6238)

HMAC-SHA1 computation over 30-second Unix epoch intervals.

## Overview
Technical cheat sheet for `totp-2fa-rfc6238` covering production deployment, configuration, and diagnostics.

## Commands and Usage
```bash
oathtool --totp -b 'BASE32SECRET'
```
