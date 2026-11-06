# TCP Sliding Window & Window Scale Option (RFC 7323)

TCP flow control prevents a fast sender from overwhelming a slow receiver's receive buffer (`rcvbuf`).

```mermaid
sequenceDiagram
    autonumber
    Sender->>Receiver: SYN (Window Scale Option = 7, Shift = 7)
    Receiver->>Sender: SYN-ACK (Window Scale Option = 7, Shift = 7)
    Sender->>Receiver: ACK (Handshake complete; scale factor 2^7 = 128)
    Sender->>Receiver: Data Segment (Seq: 1000..2000)
    Receiver->>Sender: ACK 2000, Window = 512 (Actual Window = 512 * 128 = 65,536 bytes)
```

---

## 1. The 16-Bit Header Limitation

The standard TCP header allocates 16 bits for window size:
$$	ext{Max Window} = 2^{16} - 1 = 65,535 	ext{ bytes (64 KB)}$$

On high-bandwidth, high-latency links (Long Fat Networks - LFNs), the Bandwidth-Delay Product (BDP) drastically exceeds 64KB:
$$	ext{BDP} = 	ext{Bandwidth (bits/sec)} 	imes 	ext{Round-Trip Time (sec)}$$

For a 10 Gbps link with a 50ms RTT:
$$	ext{BDP} = 10 	imes 10^9 	imes 0.050 / 8 = 62.5 	ext{ MB}$$

---

## 2. Window Scale Factor (RFC 7323)

- Shift count $S$ specified from 0 to 14.
- Actual advertised window:
  $$	ext{True Window} = 	ext{Advertised 16-bit Window} 	imes 2^S$$
- Maximum window size with $S=14$:
  $$65,535 	imes 2^{14} = 1,073,725,440 	ext{ bytes (1 GB)}$$
