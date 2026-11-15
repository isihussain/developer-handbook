# CPU Microarchitecture: Branch Prediction & TAGE Predictor

Modern speculative execution CPUs utilize Branch Target Buffers (BTB) and tagged geometric history length (TAGE) predictors to sustain instruction pipelines without pipeline stalls.

```mermaid
flowchart TD
    FetchPC[Instruction Fetch: Program Counter PC] --> BTB[Branch Target Buffer: Predicts Target Address]
    FetchPC --> TAGE[TAGE Predictor: Multiple Geometric History Tables]
    TAGE --> DirectBase[Base Bimodal Predictor]
    TAGE --> TaggedT1[Tagged Table T1: Short History]
    TAGE --> TaggedT2[Tagged Table T2: Medium History]
    TAGE --> TaggedT3[Tagged Table T3: Long History (e.g. 1000+ branches)]
    TaggedT3 -->|Provider match| Prediction[Predicted Direction: Taken / Not Taken]
```

---

## 1. Structural Elements

* **Branch Target Buffer (BTB):** Associative cache indexed by instruction address; stores target jump addresses to avoid waiting for branch instruction decoding.
* **TAGE Mechanics:** Uses multiple prediction tables indexed by branch address hashed with geometrically increasing history lengths ($L_i = lpha^{i-1} 	imes L_1$).
