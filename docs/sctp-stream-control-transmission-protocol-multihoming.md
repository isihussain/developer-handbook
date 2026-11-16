# Transport Protocols: SCTP Multi-Homing and Multi-Streaming

Stream Control Transmission Protocol (SCTP, RFC 4960) provides message-oriented, multi-homed, and multi-stream transport, powering telecom 5G signaling and WebRTC data channels.

```mermaid
flowchart TD
    Association[SCTP Association between Host A & B] --> MultiHome[Multi-Homing: Redundant IP Paths]
    MultiHome --> PrimaryPath[Primary Path: 192.168.1.10]
    MultiHome --> BackupPath[Backup Path: 10.0.0.10: Seamless failover on link drop!]
    Association --> MultiStream[Multi-Streaming: Independent logical channels]
    MultiStream --> Stream1[Stream 1: Packet loss does NOT block Stream 2!]
    MultiStream --> Stream2[Stream 2: Eliminates head-of-line blocking!]
```

---

## 1. 4-Way Cookie Handshake
SCTP uses an `INIT / INIT-ACK / COOKIE-ECHO / COOKIE-ACK` handshake to prevent SYN flood attacks by making the handshake state-free until the third flight.
