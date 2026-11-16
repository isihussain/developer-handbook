# Transport Protocols: TCP Head-of-Line (HoL) Blocking Resolution

HTTP/2 multiplexes multiple streams over a single TCP connection. However, if a single TCP packet is dropped, ALL streams stall until retransmission succeeds.

```mermaid
flowchart TD
    subgraph HTTP2_Over_TCP[HTTP/2 over Single TCP Stream]
        S1[Stream 1: CSS]
        S2[Stream 2: JPEG (PACKET LOST!)]
        S3[Stream 3: JS Payload]
        S2 -->|TCP Stalls Entire Stream!| BlockAll[Streams 1 & 3 are BLOCKED until S2 retransmits!]
    end
    subgraph HTTP3_Over_QUIC[HTTP/3 over Independent QUIC Streams]
        Q1[Stream 1: Delivered Immediately!]
        Q2[Stream 2: Retransmits independently]
        Q3[Stream 3: Delivered Immediately!]
    end
```

---

## 1. The QUIC Solution
QUIC operates over UDP where each stream has its own independent loss detection and retransmission state machine.
