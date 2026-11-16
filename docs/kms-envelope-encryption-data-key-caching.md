# Data Protection: Key Management Service (KMS) Envelope Encryption

Envelope encryption protects large volumes of data by encrypting plaintext with a local Data Encryption Key (DEK), and encrypting the DEK with a root Key Management Service (KMS) Key.

```mermaid
flowchart TD
    App[Application Worker] --> KMS[KMS: GenerateDataKey(KeyId='root_key')]
    KMS --> PlainDEK[Plaintext DEK: 256-bit AES Key]
    KMS --> EncryptedDEK[Ciphertext DEK: Encrypted by KMS Root Key]
    PlainDEK --> Encrypt[Encrypt 500GB Database Backup locally]
    Encrypt --> Output[Store Encrypted Backup + Encrypted DEK together]
    PlainDEK --> Zero[Zero memory of Plaintext DEK in RAM!]
```

---

## 1. Performance Benefits
- High-volume data encryption runs locally at full NVMe memory bus speed.
- The root KMS key never leaves hardware security modules (HSMs).
