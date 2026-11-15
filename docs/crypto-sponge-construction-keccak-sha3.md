# Cryptography: Keccak Sponge Construction & SHA-3 Internals

The Keccak sponge construction operates on a 1600-bit state array divided into bitrate $r$ and capacity $c$, alternating between absorbing input messages and squeezing output digests.

```mermaid
flowchart LR
    subgraph AbsorbingPhase[Absorbing Phase: Rate r = 1088, Capacity c = 512]
        M0[Message Block 0] --> XOR0[XOR into Rate r]
        XOR0 --> KeccakF0[Keccak-f 1600 Permutation]
        M1[Message Block 1] --> XOR1[XOR into Rate r]
        KeccakF0 --> XOR1
        XOR1 --> KeccakF1[Keccak-f 1600 Permutation]
    end
    subgraph SqueezingPhase[Squeezing Phase]
        KeccakF1 --> Out0[Extract r bits: Digest Block 0]
        KeccakF1 --> KeccakF2[Keccak-f 1600 Permutation]
        KeccakF2 --> Out1[Extract r bits: Digest Block 1]
    end
```

---

## 1. State Matrix & Round Functions

* **3D State Array:** $5 	imes 5$ lanes of 64-bit words ($5 	imes 5 	imes 64 = 1600	ext{ bits}$).
* **Five Permutation Steps:** Each of the 24 rounds executes:
  - $	heta$ (Theta): Parity diffusion across columns.
  - $ho$ (Rho): Bitwise rotation within individual lanes.
  - $\pi$ (Pi): Permutation and transposition of lane coordinates.
  - $\chi$ (Chi): Non-linear algebraic layer (providing cryptographic confusion).
  - $\iota$ (Iota): XORing round constants into lane $[0, 0]$ to break symmetry.
