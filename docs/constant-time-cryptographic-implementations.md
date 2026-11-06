# Constant-Time Cryptographic Implementations

Timing variations in secret-dependent branches, lookups, and division instructions leak cryptographic keys across cache lines and branch predictors. Constant-time engineering eliminates execution variance.

```mermaid
flowchart TD
    Secret[Secret Data: Private Keys / Plaintext] --> Rule1[Rule 1: Never Branch on Secret Condition]
    Secret --> Rule2[Rule 2: Never Index Arrays / Memory using Secret Offsets]
    Secret --> Rule3[Rule 3: Avoid Variable-Time CPU Instructions: DIV, IDIV, SQRT]
    Rule1 --> Bitmask[Conditional Select via Arithmetic Bitmask: c & a | ~c & b]
    Rule2 --> Scan[Linear Constant-Time Array Sweep]
    Rule3 --> Mult[Replace Division with Montgomery / Barret Reduction]
```

---

## 1. Constant-Time Conditional Selection Pattern

To choose between variable $a$ and $b$ based on secret condition boolean $c \in \{0, 1\}$:

```c
// VULNERABLE: Leaks c via branch predictor
int result = c ? a : b;

// CONSTANT-TIME: Bitwise multiplexer
int32_t mask = -((int32_t)c); // c=1 -> 0xFFFFFFFF, c=0 -> 0x00000000
int32_t result = (mask & a) | (~mask & b);
```

---

## 2. Constant-Time Memory Comparison

```c
int constant_time_memcmp(const void *s1, const void *s2, size_t n) {
    const uint8_t *p1 = s1;
    const uint8_t *p2 = s2;
    uint8_t diff = 0;
    for (size_t i = 0; i < n; i++) {
        diff |= (p1[i] ^ p2[i]);
    }
    return (int)diff; // diff == 0 iff buffers match identically
}
```
