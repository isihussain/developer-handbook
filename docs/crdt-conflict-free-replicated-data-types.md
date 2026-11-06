# Conflict-Free Replicated Data Types (CRDT)

CRDTs enable multi-master concurrent data replication with mathematical convergence guarantees without central locking.

---

## 1. CvRDT (State-Based) vs CmRDT (Operation-Based)

- **CvRDT**: Nodes send complete states; merge function $\sqcup$ must be commutative, associative, and idempotent (semilattice).
- **CmRDT**: Nodes broadcast operations; causal delivery layer guarantees operations execute in identical causal order.
