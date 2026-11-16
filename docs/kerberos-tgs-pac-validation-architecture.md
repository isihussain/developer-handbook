# Authentication Architecture: Kerberos v5 and Active Directory PAC

Kerberos authenticates users across enterprise networks without transmitting passwords over the wire, utilizing symmetric key cryptography and short-lived tickets.

```mermaid
sequenceDiagram
    participant Client
    participant AS as Authentication Service (KDC)
    participant TGS as Ticket-Granting Service (KDC)
    participant Server as Target Service (e.g. File Share)

    Client->>AS: AS-REQ (Encrypted Timestamp with Password Hash)
    AS-->>Client: AS-REP (Ticket-Granting Ticket TGT + Session Key)
    Client->>TGS: TGS-REQ (TGT + Authenticator)
    TGS-->>Client: TGS-REP (Service Ticket containing Privilege Attribute Certificate PAC)
    Client->>Server: AP-REQ (Service Ticket)
    Note over Server: Validates PAC signatures with KDC and grants access!
```

---

## 1. PAC Validation
The Privilege Attribute Certificate (PAC) contains user security identifiers (SIDs) and group memberships, cryptographically signed by the KDC to prevent privilege escalation.
