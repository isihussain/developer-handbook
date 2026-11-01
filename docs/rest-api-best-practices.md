# RESTful API Architecture

Idempotency (GET, PUT, DELETE), pagination headers, resource naming, and RFC 7807 error responses.

## Overview
Technical cheat sheet for `rest-api-best-practices` covering production deployment, configuration, and diagnostics.

## Commands and Usage
```bash
HTTP/1.1 200 OK
Link: <https://api.example.com/items?page=2>; rel="next"
```
