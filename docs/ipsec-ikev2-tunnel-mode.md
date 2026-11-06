# IPsec Architecture: IKEv2 & ESP Tunnel Mode

IPsec protects IP datagrams via Internet Key Exchange (IKEv2) and Encapsulating Security Payload (ESP).

```
[ ESP Tunnel Mode Packet Layout ]
+------------+------------+---------------+----------------+------------+
| New IP Hdr | ESP Header | Orig IP + Data| ESP Trailer    | ESP Auth   |
+------------+------------+---------------+----------------+------------+
                          |<-------- Encrypted ----------->|
             |<-------------------- Authenticated --------------------->|
```

---

## 1. Transport vs Tunnel Mode

- **Transport Mode**: Protects payload only; original IP header remains exposed.
- **Tunnel Mode**: Entire original packet is encapsulated in a new outer IP packet with a new header, hiding origin and destination routing.
