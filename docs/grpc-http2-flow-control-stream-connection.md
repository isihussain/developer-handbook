# RPC Networking: gRPC HTTP/2 Flow Control Windows

HTTP/2 uses window updates (`WINDOW_UPDATE` frames) at both the individual stream level and the global connection level to prevent fast senders from exhausting receiver buffers.

```mermaid
flowchart TD
    Sender[gRPC Client] --> StreamCheck{Stream Window Available?}
    StreamCheck -->|Yes| ConnCheck{Connection Window Available?}
    ConnCheck -->|Yes| SendDATA[Send HTTP/2 DATA Frame]
    ConnCheck -->|No| StallConn[Stall All Streams on Connection!]
    StreamCheck -->|No| StallStream[Stall Only This Specific Stream!]
    Receiver[gRPC Server Buffer] --> Drain[Application Reads Messages]
    Drain --> SendUpdate[Send WINDOW_UPDATE Frame]
```

---

## 1. Head-Of-Line Blocking Mitigation
A stalled stream with a full window does not block other independent streams on the shared TCP connection.
