# Cross-Origin Resource Sharing (CORS)

Access-Control-Allow-Origin, preflight OPTIONS requests, and credentialed requests.

## Overview
Technical cheat sheet for `cors-configuration` covering production deployment, configuration, and diagnostics.

## Commands and Usage
```bash
curl -H 'Origin: https://app.example.com' -H 'Access-Control-Request-Method: POST' -X OPTIONS https://api.example.com
```
