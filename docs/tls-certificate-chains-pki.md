# X.509 Certificate Chains & Public Key Infrastructure

Root CA, Intermediate CA, leaf certificates, CRL, and OCSP stapling.

## Overview
Technical cheat sheet for `tls-certificate-chains-pki` covering production deployment, configuration, and diagnostics.

## Commands and Usage
```bash
openssl verify -CAfile ca.pem cert.pem
```
