# Database Storage: Gorilla Floating-Point XOR Compression

The Facebook Gorilla time-series compression algorithm compresses IEEE 754 64-bit floating-point values into variable-length bitstreams using XOR delta encoding against preceding values.

```mermaid
flowchart TD
    ValCurrent[Current Float 64: V_i] --> XOR[XOR: D = V_i ^ V_{i-1}]
    XOR --> CheckZero{Is D == 0?}
    CheckZero -->|Yes| Bit0[Output Single Bit: 0 (Repeated Float)]
    CheckZero -->|No| Bit1[Output Control Bit: 1]
    Bit1 --> CheckSpan{Leading & Trailing Zeros match previous?}
    CheckSpan -->|Yes| CaseA["Bit '0' + Output [Bits_Prev] Variable Payload"]
    CheckSpan -->|No| CaseB["Bit '1' + 5-bit Lead count + 6-bit Length + Payload Bits"]
```

---

## 1. Encoding Mechanism

1. Compute $D = V_i \oplus V_{i-1}$.
2. If $D = 0$, store bit `0`.
3. If $D 
eq 0$, store bit `1`, followed by:
   - Control bit `0` if leading and trailing zero spans match the previous non-zero XOR delta (reusing length metadata).
   - Control bit `1` if spans differ, followed by 5 bits (leading zeros count), 6 bits (length of meaningful bits), and the meaningful bits themselves.
4. **Compression Ratio:** Typical telemetry metrics compress from 8 bytes down to an average of 1.37 bytes per float value.
