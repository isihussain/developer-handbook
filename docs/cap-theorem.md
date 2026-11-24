# CAP Theorem & PACELC Distributed Systems

CAP describes a limit on a replicated data system during a network partition: it cannot guarantee both a linearizable response from every non-failing node and a successful response from every non-failing node. A system can choose which guarantee to preserve while the partition prevents replicas from coordinating. Partition tolerance is therefore a condition distributed systems must handle, not a feature that can be switched off.

The choice is narrower than “pick any two.” When the network is healthy, a system can provide both consistency and availability. The trade-off appears when a partition prevents communication between replicas.

PACELC adds the healthy-network case: if there is a **P**artition, choose between **A**vailability and **C**onsistency; **E**lse, choose between **L**atency and **C**onsistency. For example, waiting for a remote replica can preserve a stronger consistency guarantee but increase response latency.

## Terms

- **Consistency:** operations behave as if they took effect atomically in one global order (linearizability).
- **Availability:** every request to a non-failing node eventually receives a successful response.
- **Partition tolerance:** the system continues operating despite messages being lost or delayed between nodes.

These definitions matter: weaker notions such as eventual consistency and availability definitions that allow error responses change the question being analyzed.

## References

- Gilbert and Lynch, [Brewer's conjecture and the feasibility of consistent, available, partition-tolerant web services](https://doi.org/10.1145/564585.564601).
- Daniel Abadi, [Consistency tradeoffs in modern distributed database system design](https://doi.org/10.14778/2367502.2367503).
