# Network Security: IEEE 802.1AE MACsec Link-Layer Encryption

MACsec (Media Access Control Security) operates at Layer 2 (Data Link Layer) to encrypt and authenticate all Ethernet network traffic between point-to-point switches, routers, and host network cards at line rate.

```mermaid
flowchart LR
    StandardFrame[Standard Ethernet: DA | SA | EtherType | Payload | FCS] --> MACsecFrame
    subgraph MACsecFrame[MACsec Encrypted Ethernet Frame]
        DA[DA: Dest MAC]
        SA[SA: Src MAC]
        SecTAG[SecTAG: EtherType 0x88E5 | TCI | AN | SL | PN | SCI]
        EncPayload[Encrypted Payload: AES-GCM-128/256]
        ICV[ICV: Integrity Check Value (16 bytes)]
        FCS[FCS: Frame Check Sequence]
    end
    DA --> SA --> SecTAG --> EncPayload --> ICV --> FCS
```

---

## 1. Frame Architecture & Key Exchange

* **SecTAG Header:** Contains Security Channel Identifier (SCI), Packet Number (PN) for anti-replay defense, and Association Number (AN).
* **Layer 2 Line-Rate Offload:** Executed in ASIC switch hardware; encrypts payload while preserving Layer 2 source and destination MAC headers for local switch forwarding.
* **MKA Protocol (802.1X):** MACsec Key Agreement protocol handles periodic rotation of Secure Association Keys (SAK) before 32-bit packet number overflow occurs.
