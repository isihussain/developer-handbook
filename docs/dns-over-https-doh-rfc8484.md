# DNS over HTTPS (DoH RFC 8484) Architecture

DoH encapsulates DNS queries and responses within standard HTTP/2 and HTTP/3 HTTPS streams over TCP/UDP port 443.

---

## 1. Mechanics

- **Media Type**: `application/dns-message`.
- **Method**: HTTP `GET` with Base64URL-encoded query parameter, or HTTP `POST` with binary body.
- **Privacy Benefit**: Prevents local ISP DNS spoofing, interception, and cleartext metadata leakage.
