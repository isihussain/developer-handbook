# QUIC Latency Observation: The Spin Bit (RFC 9000)

Enables passive round-trip latency measurements by network middleboxes without decrypting payload packets.

---

## 1. Mechanics

- Client spins (inverts) the spin bit once per RTT period.
- Server reflects the observed spin bit back to the client.
- Network monitoring taps measure the time between edge flips to calculate link RTT.
