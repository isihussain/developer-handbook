# Constant-Time Montgomery Ladder Architecture

The Montgomery Ladder computes scalar multiplication $Q = [k]P$ on Montgomery curves (Curve25519 / X25519) using only $X$- and $Z$-coordinates, maintaining strictly uniform memory access and branch patterns.

```mermaid
flowchart TD
    Scalar[Scalar k: 256 bits clamped] --> Loop[Iterate bit by bit from bit 254 down to 0]
    Loop --> CSwap[Conditional Swap CSWAP based on current bit]
    CSwap --> DiffAdd[Differential Addition: XADD]
    DiffAdd --> Double[Point Doubling: XDBL]
    Double --> CSwap2[Undo Conditional Swap CSWAP]
    CSwap2 --> Loop
```

---

## 1. Branchless Conditional Swap (`CSWAP`)

```c
void cswap(uint32_t b, felem *p, felem *q) {
    uint32_t mask = -b; // 0xFFFFFFFF if b=1, 0x00000000 if b=0
    for (int i = 0; i < 5; ++i) {
        uint32_t dummy = mask & (p->v[i] ^ q->v[i]);
        p->v[i] ^= dummy;
        q->v[i] ^= dummy;
    }
}
```
