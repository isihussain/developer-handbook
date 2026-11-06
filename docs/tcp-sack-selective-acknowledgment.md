# TCP Selective Acknowledgment (SACK RFC 2018)

Mitigates multiple packet drops in a single window by transmitting explicit contiguous received byte ranges.

---

## 1. Header Option Layout

```
+--------+--------+----------------+----------------+
| Kind=5 | Length | Left Edge Block| Right Edge Blk |
+--------+--------+----------------+----------------+
```
Sender maintains a `pipe` variable counting in-flight packets, retransmitting only unacknowledged holes rather than the entire sliding window.
