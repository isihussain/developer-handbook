# Developer Handbook

An authoritative, production-grade systems engineering handbook, cheat sheet repository, and low-latency architecture guide. Contains **264 technical cheat sheets** covering Linux internals, high-frequency trading (HFT), distributed consensus, zero-knowledge proofs, networking, cryptography, and compiler engineering.

Online Documentation: **[https://isihussain.github.io/developer-handbook/](https://isihussain.github.io/developer-handbook/)**

---

## Technical Domains Covered

1. **High-Frequency Trading & Low-Latency Systems**: Kernel bypass (DPDK, OpenOnload, AF_XDP), LOB matching engines, FIX/ITCH binary decoders, lock-free SPSC queues, Disruptor pattern, cache coherence (MESI/MOESI), thread affinity, Intel CAT, and 1GB hugepages.
2. **Linux Kernel & Systems Programming**: Memory management, eBPF/XDP, cgroups v2, page cache writeback, epoll vs io_uring, signal safety, and system call tracing.
3. **Distributed Systems & Consensus**: Raft, Multi-Paxos, PBFT, Chandy-Lamport snapshots, Spanner TrueTime, Vector Clocks, Calvin deterministic transactions, and CRDTs.
4. **Zero-Knowledge Systems & Advanced Cryptography**: Plonk arithmetization, FRI/STARKs, KZG polynomial commitments, Bulletproofs, BLS12-381, FROST threshold signatures, and Post-Quantum Cryptography (ML-KEM, ML-DSA, SLH-DSA).
5. **Database Storage Engines**: LSM-Trees, B+ Trees, SSTable block formats, MVCC, Serializable Snapshot Isolation (SSI), and RocksDB compaction tuning.
6. **Compilers & Runtime Systems**: AST lowering, SSA IR construction, register allocation (Graph Coloring / Linear Scan), and tracing JIT compilation.
7. **Computer Networking & Protocols**: BGP EVPN/VXLAN, TCP congestion control (BBR vs CUBIC), QUIC/HTTP3, IEEE 1588 PTP, and RoCEv2 RDMA.
8. **Cloud Architecture & Security**: TLS 1.3 0-RTT, WireGuard cryptokey routing, OAuth 2.0 / OIDC, SAML SSO, and Confidential Computing (AMD SEV-SNP).

---

## Complete Catalog (264 Cheat Sheets)

| # | Topic | Document |
|---|---|---|
| 1 | [AES-256-GCM Authenticated Encryption](docs/aes-gcm-authenticated-encryption.md) | `aes-gcm-authenticated-encryption.md` |
| 2 | [ARP Spoofing & Dynamic ARP Inspection (DAI)](docs/arp-spoofing-mitigation.md) | `arp-spoofing-mitigation.md` |
| 3 | [Asymmetric Key Cryptography](docs/asymmetric-cryptography.md) | `asymmetric-cryptography.md` |
| 4 | [B-Tree vs B+ Tree Storage Engine Architecture](docs/b-tree-vs-b-plus-tree-internal-nodes.md) | `b-tree-vs-b-plus-tree-internal-nodes.md` |
| 5 | [Defensive Bash Scripting](docs/bash-scripting.md) | `bash-scripting.md` |
| 6 | [Argon2id & Bcrypt Password Hashing](docs/bcrypt-argon2-password-hashing.md) | `bcrypt-argon2-password-hashing.md` |
| 7 | [BGP EVPN & VXLAN Data Center Overlay Architecture](docs/bgp-evpn-vxlan-overlay-networks.md) | `bgp-evpn-vxlan-overlay-networks.md` |
| 8 | [Scaling iBGP: Route Reflectors & AS Confederations](docs/bgp-route-reflectors-confederations.md) | `bgp-route-reflectors-confederations.md` |
| 9 | [Border Gateway Protocol (BGP) Routing](docs/bgp-routing-fundamentals.md) | `bgp-routing-fundamentals.md` |
| 10 | [Advanced Probabilistic Filters: Counting & Cuckoo Filters](docs/bloom-filters-counting-and-cuckoo.md) | `bloom-filters-counting-and-cuckoo.md` |
| 11 | [Bloom Filters & Count-Min Sketches](docs/bloom-filters-probabilistic.md) | `bloom-filters-probabilistic.md` |
| 12 | [CPU Branch Prediction & Inter-Processor TLB Shootdowns](docs/branch-prediction-tlb-shootdowns.md) | `branch-prediction-tlb-shootdowns.md` |
| 13 | [Practical Byzantine Fault Tolerance (PBFT)](docs/byzantine-fault-tolerance-pbft.md) | `byzantine-fault-tolerance-pbft.md` |
| 14 | [Calvin Deterministic Transaction Processing Protocol](docs/calvin-deterministic-distributed-transactions.md) | `calvin-deterministic-distributed-transactions.md` |
| 15 | [CAP Theorem & PACELC Distributed Systems](docs/cap-theorem.md) | `cap-theorem.md` |
| 16 | [Linux cgroup v2 Memory Controller & OOM Scoring](docs/cgroup-v2-memory-protection.md) | `cgroup-v2-memory-protection.md` |
| 17 | [RFC 8439 ChaCha20-Poly1305 AEAD Architecture](docs/chacha20-poly1305-aead-construction.md) | `chacha20-poly1305-aead-construction.md` |
| 18 | [Chandy-Lamport Distributed Snapshot Algorithm](docs/chandy-lamport-distributed-snapshots.md) | `chandy-lamport-distributed-snapshots.md` |
| 19 | [Chaumian Blind Signatures & Cryptographic e-Cash](docs/chaumian-blind-signatures-ecash.md) | `chaumian-blind-signatures-ecash.md` |
| 20 | [Circuit Breaker Pattern in Microservices](docs/circuit-breaker-resilience.md) | `circuit-breaker-resilience.md` |
| 21 | [Apache Parquet Columnar Storage Encodings](docs/columnar-storage-parquet-encodings.md) | `columnar-storage-parquet-encodings.md` |
| 22 | [Compiler AST Lowering & IR Generation](docs/compiler-ast-lowering.md) | `compiler-ast-lowering.md` |
| 23 | [Static Single Assignment (SSA) Form](docs/compiler-intermediate-representation-ssa.md) | `compiler-intermediate-representation-ssa.md` |
| 24 | [Confidential Computing: AMD SEV-SNP & Intel TDX](docs/confidential-computing-enclaves-sev.md) | `confidential-computing-enclaves-sev.md` |
| 25 | [Consistent Hashing with Bounded Loads](docs/consistent-hashing-bounded-loads.md) | `consistent-hashing-bounded-loads.md` |
| 26 | [Consistent Hashing & Distributed Hash Tables](docs/consistent-hashing-dht.md) | `consistent-hashing-dht.md` |
| 27 | [Constant-Time Cryptographic Implementations](docs/constant-time-cryptographic-implementations.md) | `constant-time-cryptographic-implementations.md` |
| 28 | [CPU Core Pinning, isolcpus & Thread Affinity](docs/core-pinning-isolcpus-thread-affinity.md) | `core-pinning-isolcpus-thread-affinity.md` |
| 29 | [Coroutine Internals: Stackful vs Stackless Models](docs/coroutine-internals-stackful-vs-stackless.md) | `coroutine-internals-stackful-vs-stackless.md` |
| 30 | [Cross-Origin Resource Sharing (CORS)](docs/cors-configuration.md) | `cors-configuration.md` |
| 31 | [C++20 Memory Model and Atomics Reference](docs/cpp20-memory-model-sequential-consistency.md) | `cpp20-memory-model-sequential-consistency.md` |
| 32 | [CPU Cache Coherence: MESI & MOESI Protocols](docs/cpu-cache-coherence-mesi-moesi.md) | `cpu-cache-coherence-mesi-moesi.md` |
| 33 | [MESI CPU Cache Coherence Protocol](docs/cpu-cache-coherence-mesi-protocol.md) | `cpu-cache-coherence-mesi-protocol.md` |
| 34 | [Microarchitectural Vulnerabilities & Speculation Barriers](docs/cpu-speculative-execution-spectre-mitigations.md) | `cpu-speculative-execution-spectre-mitigations.md` |
| 35 | [CPU TLB Performance: 2MB vs 1GB HugePages](docs/cpu-tlb-hugepages-performance.md) | `cpu-tlb-hugepages-performance.md` |
| 36 | [Conflict-Free Replicated Data Types (CRDT)](docs/crdt-conflict-free-replicated-data-types.md) | `crdt-conflict-free-replicated-data-types.md` |
| 37 | [Cron Schedule Patterns](docs/cron-syntax.md) | `cron-syntax.md` |
| 38 | [Cryptographic Hash Functions](docs/crypto-hash-functions.md) | `crypto-hash-functions.md` |
| 39 | [Ed25519 Digital Signature Scheme (RFC 8032)](docs/cryptographic-signatures-ed25519.md) | `cryptographic-signatures-ed25519.md` |
| 40 | [cURL Command Reference](docs/curl-cheatsheet.md) | `curl-cheatsheet.md` |
| 41 | [Database ACID Transaction Properties](docs/database-acid-properties.md) | `database-acid-properties.md` |
| 42 | [PostgreSQL MVCC: Vacuum Freeze & Transaction Wraparound](docs/database-mvcc-vacuum-freeze.md) | `database-mvcc-vacuum-freeze.md` |
| 43 | [Database Sharding Architectures](docs/database-sharding-patterns.md) | `database-sharding-patterns.md` |
| 44 | [Deterministic Replay Architecture for Trading Engines](docs/deterministic-replay-trading-systems.md) | `deterministic-replay-trading-systems.md` |
| 45 | [Differential Privacy & Laplace Noise Calibration](docs/differential-privacy-laplace-mechanism.md) | `differential-privacy-laplace-mechanism.md` |
| 46 | [Diffie-Hellman Key Exchange (ECDH)](docs/diffie-hellman-key-exchange.md) | `diffie-hellman-key-exchange.md` |
| 47 | [LMAX Disruptor High-Throughput Ring Buffer](docs/disruptor-ring-buffer-architecture.md) | `disruptor-ring-buffer-architecture.md` |
| 48 | [LMAX Disruptor Architecture Pattern](docs/disruptor-ring-buffer-pattern.md) | `disruptor-ring-buffer-pattern.md` |
| 49 | [Distributed Deadlock Detection & Edge Chasing](docs/distributed-deadlock-detection-wait-for-graph.md) | `distributed-deadlock-detection-wait-for-graph.md` |
| 50 | [Distributed Locking with Redlock Algorithm](docs/distributed-locking-redlock.md) | `distributed-locking-redlock.md` |
| 51 | [Database Query Execution: Volcano Iterator Model](docs/distributed-query-optimization-volcano-model.md) | `distributed-query-optimization-volcano-model.md` |
| 52 | [OpenTelemetry Distributed Tracing](docs/distributed-tracing-opentelemetry.md) | `distributed-tracing-opentelemetry.md` |
| 53 | [Two-Phase Commit Protocol (2PC)](docs/distributed-transactions-2pc.md) | `distributed-transactions-2pc.md` |
| 54 | [Direct Memory Access (DMA) & Bounce Buffers](docs/dma-direct-memory-access-bounce-buffers.md) | `dma-direct-memory-access-bounce-buffers.md` |
| 55 | [DNSSEC NSEC3 Authenticated Denial of Existence (RFC 5155)](docs/dns-nsec3-zone-walking-mitigation.md) | `dns-nsec3-zone-walking-mitigation.md` |
| 56 | [DNS over HTTPS (DoH RFC 8484) Architecture](docs/dns-over-https-doh-rfc8484.md) | `dns-over-https-doh-rfc8484.md` |
| 57 | [DNS Record Types & Resolution](docs/dns-record-types.md) | `dns-record-types.md` |
| 58 | [DNSSEC Cryptographic Validation](docs/dnssec-validation.md) | `dnssec-validation.md` |
| 59 | [Docker Compose Services](docs/docker-compose.md) | `docker-compose.md` |
| 60 | [Docker Quick Reference](docs/docker-guide.md) | `docker-guide.md` |
| 61 | [Docker Multi-Stage Build Optimization](docs/docker-multistage-builds.md) | `docker-multistage-builds.md` |
| 62 | [Double Ratchet Protocol (Signal Protocol)](docs/double-ratchet-signal-protocol.md) | `double-ratchet-signal-protocol.md` |
| 63 | [ELF Dynamic Linking: PLT and GOT](docs/dynamic-linker-elf-plt-got.md) | `dynamic-linker-elf-plt-got.md` |
| 64 | [Extended Berkeley Packet Filter (eBPF)](docs/ebpf-basics.md) | `ebpf-basics.md` |
| 65 | [Dynamic Tracing: eBPF kprobes vs Static Kernel Tracepoints](docs/ebpf-kprobes-and-tracepoints.md) | `ebpf-kprobes-and-tracepoints.md` |
| 66 | [eBPF Express Data Path (XDP) Architecture](docs/ebpf-xdp-high-speed-packet-filtering.md) | `ebpf-xdp-high-speed-packet-filtering.md` |
| 67 | [BLS12-381 & BN254 Elliptic Curve Pairings](docs/elliptic-curve-pairings-bls12381.md) | `elliptic-curve-pairings-bls12381.md` |
| 68 | [Environment Variables Guide](docs/environment-variables.md) | `environment-variables.md` |
| 69 | [Epoch-Based Reclamation (EBR) Architecture](docs/epoch-based-reclamation-ebr.md) | `epoch-based-reclamation-ebr.md` |
| 70 | [Epoch-Based Reclamation & Userspace RCU](docs/epoch-based-reclamation-rcu-userspace.md) | `epoch-based-reclamation-rcu-userspace.md` |
| 71 | [Linux epoll: Level-Triggered vs Edge-Triggered Semantics](docs/epoll-level-triggered-vs-edge-triggered.md) | `epoll-level-triggered-vs-edge-triggered.md` |
| 72 | [Linux epoll vs. io_uring for Trading Gateways](docs/epoll-vs-io-uring-high-throughput.md) | `epoll-vs-io-uring-high-throughput.md` |
| 73 | [Command Query Responsibility Segregation (CQRS)](docs/event-driven-cqrs-architecture.md) | `event-driven-cqrs-architecture.md` |
| 74 | [Execution Algorithms: VWAP, TWAP & POV Architecture](docs/execution-algorithms-vwap-twap-pov.md) | `execution-algorithms-vwap-twap-pov.md` |
| 75 | [False Sharing & Cache Line Bouncing Elimination](docs/false-sharing-cache-line-bouncing.md) | `false-sharing-cache-line-bouncing.md` |
| 76 | [Zero-Allocation FIX Protocol Parsing Architecture](docs/fix-protocol-parser-zero-allocation.md) | `fix-protocol-parser-zero-allocation.md` |
| 77 | [Differential Flame Graphs Profiling](docs/flamegraphs-differential-profiling.md) | `flamegraphs-differential-profiling.md` |
| 78 | [FPGA-NIC Co-Processing & Bump-in-the-Wire Architecture](docs/fpga-nic-co-processing-ultra-low-latency.md) | `fpga-nic-co-processing-ultra-low-latency.md` |
| 79 | [CKKS & BFV Fully Homomorphic Encryption (FHE)](docs/fully-homomorphic-encryption-ckks.md) | `fully-homomorphic-encryption-ckks.md` |
| 80 | [Garbage Collection: Mark-and-Sweep & Tri-Color](docs/garbage-collection-mark-sweep.md) | `garbage-collection-mark-sweep.md` |
| 81 | [JDK ZGC: Colored Pointers & Load Barriers](docs/garbage-collection-zgc-colored-pointers.md) | `garbage-collection-zgc-colored-pointers.md` |
| 82 | [GNU Debugger (GDB) Reference](docs/gdb-cheatsheet.md) | `gdb-cheatsheet.md` |
| 83 | [Interactive Git Rebase](docs/git-rebase.md) | `git-rebase.md` |
| 84 | [Git Stash Cheat Sheet](docs/git-stash.md) | `git-stash.md` |
| 85 | [Git Workflow Guidelines](docs/git-workflow.md) | `git-workflow.md` |
| 86 | [GitHub Actions Advanced Caching & Matrix Builds](docs/github-actions-caching-matrix.md) | `github-actions-caching-matrix.md` |
| 87 | [Epidemic Gossip Protocols & SWIM Failure Detection](docs/gossip-protocol-epidemic-dissemination.md) | `gossip-protocol-epidemic-dissemination.md` |
| 88 | [SWIM Protocol & Distributed Failure Detection](docs/gossip-protocol-swim.md) | `gossip-protocol-swim.md` |
| 89 | [Grafana Declarative Dashboard Provisioning](docs/grafana-dashboard-provisioning.md) | `grafana-dashboard-provisioning.md` |
| 90 | [Text Processing Essentials](docs/grep-awk-sed.md) | `grep-awk-sed.md` |
| 91 | [Groth16 zk-SNARK Protocol Synthesis](docs/groth16-zero-knowledge-synthesis.md) | `groth16-zero-knowledge-synthesis.md` |
| 92 | [gRPC & Protocol Buffers v3](docs/grpc-protocol-buffers.md) | `grpc-protocol-buffers.md` |
| 93 | [Hardware Branch Prediction & Speculative Execution](docs/hardware-branch-prediction-btb-pht.md) | `hardware-branch-prediction-btb-pht.md` |
| 94 | [Hardware Security Modules (HSM) & PKCS#11 Architecture](docs/hardware-security-modules-pkcs11.md) | `hardware-security-modules-pkcs11.md` |
| 95 | [Hazard Pointers for Lock-Free Memory Reclamation](docs/hazard-pointers-lock-free-memory-reclamation.md) | `hazard-pointers-lock-free-memory-reclamation.md` |
| 96 | [Helm v3 Package Management & Go Templating](docs/helm-charts-templating.md) | `helm-charts-templating.md` |
| 97 | [HMAC-SHA512 Webhook & API Request Signing](docs/hmac-sha512-api-signing.md) | `hmac-sha512-api-signing.md` |
| 98 | [HTTP Status Codes](docs/http-status-codes.md) | `http-status-codes.md` |
| 99 | [Hugepages (2MB / 1GB) & TLB Miss Elimination](docs/hugepages-1gb-2mb-tlb-miss-elimination.md) | `hugepages-1gb-2mb-tlb-miss-elimination.md` |
| 100 | [Hybrid Logical Clocks (HLC) Architecture](docs/hybrid-logical-clocks-hlc.md) | `hybrid-logical-clocks-hlc.md` |
| 101 | [HyperLogLog (HLL) Cardinality Estimation](docs/hyperloglog-cardinality-estimation.md) | `hyperloglog-cardinality-estimation.md` |
| 102 | [Inner Product Arguments & Bulletproofs](docs/inner-product-arguments-bulletproofs.md) | `inner-product-arguments-bulletproofs.md` |
| 103 | [Intel CAT (Cache Allocation Technology) Partitioning](docs/intel-cat-cache-allocation-technology.md) | `intel-cat-cache-allocation-technology.md` |
| 104 | [IP Multicast & IGMP Protocols](docs/ip-multicast-igmp.md) | `ip-multicast-igmp.md` |
| 105 | [IPsec Architecture: IKEv2 & ESP Tunnel Mode](docs/ipsec-ikev2-tunnel-mode.md) | `ipsec-ikev2-tunnel-mode.md` |
| 106 | [NASDAQ ITCH & OUCH Binary Protocols](docs/itch-ouch-nasdaq-binary-protocols.md) | `itch-ouch-nasdaq-binary-protocols.md` |
| 107 | [Just-In-Time (JIT) Tracing & Deoptimization](docs/jit-compilation-tracing.md) | `jit-compilation-tracing.md` |
| 108 | [jq JSON Processor](docs/jq-guide.md) | `jq-guide.md` |
| 109 | [JSON Web Tokens (JWT)](docs/jwt-tokens.md) | `jwt-tokens.md` |
| 110 | [Apache Kafka Partitioning & Consumer Groups](docs/kafka-partitions-consumer-groups.md) | `kafka-partitions-consumer-groups.md` |
| 111 | [AF_XDP (XSK) Zero-Copy Network Ingress Architecture](docs/kernel-bypass-af-xdp-zero-copy.md) | `kernel-bypass-af-xdp-zero-copy.md` |
| 112 | [Kernel Bypass with DPDK Poll Mode Drivers (PMD)](docs/kernel-bypass-dpdk-poll-mode-drivers.md) | `kernel-bypass-dpdk-poll-mode-drivers.md` |
| 113 | [Solarflare OpenOnload & Kernel Bypass Architecture](docs/kernel-bypass-solarflare-openonload.md) | `kernel-bypass-solarflare-openonload.md` |
| 114 | [Linux Kernel SLUB Allocator Architecture](docs/kernel-memory-slub-allocator.md) | `kernel-memory-slub-allocator.md` |
| 115 | [Linux Kernel Page Fault Handling & Copy-on-Write (COW)](docs/kernel-page-fault-handling-cow.md) | `kernel-page-fault-handling-cow.md` |
| 116 | [Kubernetes Ingress Controllers & Gateway API](docs/kubernetes-ingress-controllers.md) | `kubernetes-ingress-controllers.md` |
| 117 | [Kubernetes Pod Lifecycle & Probes](docs/kubernetes-pod-lifecycle.md) | `kubernetes-pod-lifecycle.md` |
| 118 | [Kubernetes Services & kube-proxy (iptables vs IPVS)](docs/kubernetes-services-kube-proxy.md) | `kubernetes-services-kube-proxy.md` |
| 119 | [KZG (Kate-Zaverucha-Goldberg) Polynomial Commitments](docs/kzg-polynomial-commitments.md) | `kzg-polynomial-commitments.md` |
| 120 | [Lattice Cryptography & Learning With Errors (LWE)](docs/lattice-cryptography-learning-with-errors.md) | `lattice-cryptography-learning-with-errors.md` |
| 121 | [Lexical Analysis: NFAs to DFAs (Thompson & Subset)](docs/lexical-analysis-dfa-nfa.md) | `lexical-analysis-dfa-nfa.md` |
| 122 | [Limit Order Book Matching Engine Architecture](docs/limit-order-book-matching-engine-design.md) | `limit-order-book-matching-engine-design.md` |
| 123 | [Linux Audit Subsystem (auditd) & Kernel Syscall Rules](docs/linux-auditd-security-event-logging.md) | `linux-auditd-security-event-logging.md` |
| 124 | [Linux Capabilities (cap_net_bind_service)](docs/linux-capabilities-privileges.md) | `linux-capabilities-privileges.md` |
| 125 | [Linux cgroup v2 I/O Controller & Throttling](docs/linux-cgroup-v2-io-controller.md) | `linux-cgroup-v2-io-controller.md` |
| 126 | [Linux Cgroups v2 CPU Throttling](docs/linux-cgroups-cpu-throttling.md) | `linux-cgroups-cpu-throttling.md` |
| 127 | [Linux Core Dump Configuration & GDB](docs/linux-core-dumps-gdb.md) | `linux-core-dumps-gdb.md` |
| 128 | [eBPF Traffic Control (tc / cls_bpf) Architecture](docs/linux-ebpf-tc-traffic-control.md) | `linux-ebpf-tc-traffic-control.md` |
| 129 | [File Descriptor Limits (ulimit & sysctl)](docs/linux-file-descriptors-limits.md) | `linux-file-descriptors-limits.md` |
| 130 | [Linux Fast Userspace Mutex (futex) Architecture](docs/linux-futex-fast-userspace-mutex.md) | `linux-futex-fast-userspace-mutex.md` |
| 131 | [Linux Security Hardening (CIS Benchmark)](docs/linux-hardening-benchmarks.md) | `linux-hardening-benchmarks.md` |
| 132 | [Linux Transparent Hugepages (THP)](docs/linux-hugepages-memory.md) | `linux-hugepages-memory.md` |
| 133 | [Linux io_uring High-Performance Asynchronous Architecture](docs/linux-io-uring-async.md) | `linux-io-uring-async.md` |
| 134 | [Linux Kernel Live Patching Architecture (klp)](docs/linux-kernel-livepatching.md) | `linux-kernel-livepatching.md` |
| 135 | [Linux Landlock LSM Sandboxing (Kernel 5.13+)](docs/linux-landlock-sandboxing.md) | `linux-landlock-sandboxing.md` |
| 136 | [Linux Namespaces and Cgroups v2](docs/linux-namespaces-cgroups.md) | `linux-namespaces-cgroups.md` |
| 137 | [Linux Network Namespaces & Virtual Ethernet (veth) Pairs](docs/linux-namespaces-network-isolation.md) | `linux-namespaces-network-isolation.md` |
| 138 | [Linux nftables Architecture & Bytecode Evaluation](docs/linux-nf-tables-firewall-bytecode.md) | `linux-nf-tables-firewall-bytecode.md` |
| 139 | [Linux Page Cache Writeback Architecture](docs/linux-page-cache-dirty-ratio.md) | `linux-page-cache-dirty-ratio.md` |
| 140 | [Linux perf & Hardware PMU Counter Profiling](docs/linux-perf-hardware-counter-sampling.md) | `linux-perf-hardware-counter-sampling.md` |
| 141 | [Linux File Permissions](docs/linux-permissions.md) | `linux-permissions.md` |
| 142 | [Linux Kernel sched_ext Extensible eBPF Scheduler](docs/linux-sched-ext-bpf-scheduler.md) | `linux-sched-ext-bpf-scheduler.md` |
| 143 | [Linux Secure Computing Mode (seccomp)](docs/linux-seccomp-bpf-filters.md) | `linux-seccomp-bpf-filters.md` |
| 144 | [Linux Process Signals](docs/linux-signals.md) | `linux-signals.md` |
| 145 | [Linux TCP CUBIC Congestion Control Architecture](docs/linux-tcp-congestion-control-cubic.md) | `linux-tcp-congestion-control-cubic.md` |
| 146 | [Linux VFS Directory Entry (dentry) Cache & Path Walk](docs/linux-vfs-dentry-cache.md) | `linux-vfs-dentry-cache.md` |
| 147 | [Virtual Memory & Memory-Mapped I/O (mmap)](docs/linux-virtual-memory-mmap.md) | `linux-virtual-memory-mmap.md` |
| 148 | [LLVM Intermediate Representation (IR)](docs/llvm-ir-fundamentals.md) | `llvm-ir-fundamentals.md` |
| 149 | [Lock-Free Data Structures & Compare-And-Swap (CAS)](docs/lock-free-atomic-cas.md) | `lock-free-atomic-cas.md` |
| 150 | [Michael & Scott Lock-Free Queue Algorithm](docs/lock-free-queue-michael-scott.md) | `lock-free-queue-michael-scott.md` |
| 151 | [Lock-Free SPSC Ring Buffer Architecture](docs/lock-free-spsc-ring-buffer-architecture.md) | `lock-free-spsc-ring-buffer-architecture.md` |
| 152 | [Log-Structured Merge (LSM) Trees Compaction Strategies](docs/log-structured-merge-trees-compaction.md) | `log-structured-merge-trees-compaction.md` |
| 153 | [Log-Structured Merge-Trees (LSM) vs B-Trees](docs/lsm-trees-vs-b-trees.md) | `lsm-trees-vs-b-trees.md` |
| 154 | [Makefile Build Recipes](docs/makefile-basics.md) | `makefile-basics.md` |
| 155 | [Markdown Syntax Reference](docs/markdown-guide.md) | `markdown-guide.md` |
| 156 | [A/B Multicast Feed Line Arbitration Architecture](docs/market-data-multicast-udp-arbitration.md) | `market-data-multicast-udp-arbitration.md` |
| 157 | [Avellaneda-Stoikov Market Making & Inventory Skew](docs/market-making-inventory-skew-avellaneda-stoikov.md) | `market-making-inventory-skew-avellaneda-stoikov.md` |
| 158 | [Hardware Memory Models: Acquire-Release Semantics](docs/memory-ordering-acquire-release.md) | `memory-ordering-acquire-release.md` |
| 159 | [Memory Models & Hardware Memory Barriers (x86 vs ARM)](docs/memory-ordering-memory-barriers-x86-arm.md) | `memory-ordering-memory-barriers-x86-arm.md` |
| 160 | [Lock-Free Memory Reclamation: Hazard Pointers](docs/memory-reclamation-hazard-pointers.md) | `memory-reclamation-hazard-pointers.md` |
| 161 | [Merkle Mountain Ranges (MMR) & Accumulators](docs/merkle-mountain-ranges-vector-commitments.md) | `merkle-mountain-ranges-vector-commitments.md` |
| 162 | [Path MTU Discovery & TCP MSS Clamping](docs/mtu-mss-path-discovery.md) | `mtu-mss-path-discovery.md` |
| 163 | [Serializable Snapshot Isolation (SSI) Architecture](docs/multi-version-concurrency-control-ssi.md) | `multi-version-concurrency-control-ssi.md` |
| 164 | [NAT Traversal: STUN, TURN, and ICE](docs/nat-traversal-stun-turn-ice.md) | `nat-traversal-stun-turn-ice.md` |
| 165 | [Nginx Reverse Proxy](docs/nginx-reverse-proxy.md) | `nginx-reverse-proxy.md` |
| 166 | [Non-Blocking Synchronization: CAS, FAA & ABA Mitigation](docs/non-blocking-atomic-primitives-cas-faa.md) | `non-blocking-atomic-primitives-cas-faa.md` |
| 167 | [NVMe-over-Fabrics (NVMe-oF): RDMA vs TCP Transport](docs/nvme-over-fabrics-rdma-tcp.md) | `nvme-over-fabrics-rdma-tcp.md` |
| 168 | [OAuth 2.0 & OIDC Authorization](docs/oauth2-grant-types.md) | `oauth2-grant-types.md` |
| 169 | [Path ORAM (Oblivious RAM) Architecture](docs/oblivious-ram-path-oram.md) | `oblivious-ram-path-oram.md` |
| 170 | [Apache Parquet Bloom Filter Predicate Pushdown](docs/parquet-bloom-filter-pushdown.md) | `parquet-bloom-filter-pushdown.md` |
| 171 | [Parsing Algorithms: LL(k), LR(k), and LALR(1)](docs/parser-lalr-vs-llk.md) | `parser-lalr-vs-llk.md` |
| 172 | [Paxos Distributed Consensus Protocol](docs/paxos-distributed-consensus-protocol.md) | `paxos-distributed-consensus-protocol.md` |
| 173 | [Pedersen Verifiable Secret Sharing (VSS)](docs/pedersen-verifiable-secret-sharing.md) | `pedersen-verifiable-secret-sharing.md` |
| 174 | [Asynchronous I/O: POSIX AIO vs Linux io_uring](docs/posix-aio-vs-io-uring.md) | `posix-aio-vs-io-uring.md` |
| 175 | [POSIX Shared Memory (shm_open)](docs/posix-shared-memory-shm.md) | `posix-shared-memory-shm.md` |
| 176 | [NIST FIPS 204 ML-DSA (Module-Lattice Digital Signatures)](docs/post-quantum-cryptography-ml-dsa.md) | `post-quantum-cryptography-ml-dsa.md` |
| 177 | [NIST FIPS 203 ML-KEM (Module-Lattice KEM)](docs/post-quantum-cryptography-ml-kem.md) | `post-quantum-cryptography-ml-kem.md` |
| 178 | [NIST FIPS 204 ML-DSA (Dilithium) Lattice Signatures](docs/post-quantum-dilithium-ml-dsa.md) | `post-quantum-dilithium-ml-dsa.md` |
| 179 | [NIST FIPS 203 ML-KEM (Kyber) Lattice Key Encapsulation](docs/post-quantum-kyber-ml-kem.md) | `post-quantum-kyber-ml-kem.md` |
| 180 | [NIST FIPS 205 SLH-DSA (SPHINCS+) Stateless Hash Signatures](docs/post-quantum-sphincs-plus-slh-dsa.md) | `post-quantum-sphincs-plus-slh-dsa.md` |
| 181 | [PostgreSQL Core Operations](docs/postgresql-basics.md) | `postgresql-basics.md` |
| 182 | [PostgreSQL Declarative Table Partitioning](docs/postgresql-partitioning-declarative.md) | `postgresql-partitioning-declarative.md` |
| 183 | [PostgreSQL MVCC & Autovacuum Tuning](docs/postgresql-vacuum-mvcc.md) | `postgresql-vacuum-mvcc.md` |
| 184 | [PostgreSQL Write-Ahead Logging (WAL) & Streaming](docs/postgresql-wal-replication.md) | `postgresql-wal-replication.md` |
| 185 | [Price-Time Priority (FIFO) Matching Algorithm](docs/price-time-priority-fifo-matching.md) | `price-time-priority-fifo-matching.md` |
| 186 | [PromQL Metrics & Alerting Engine](docs/prometheus-promql-metrics.md) | `prometheus-promql-metrics.md` |
| 187 | [IEEE 1588 PTP & Sub-Microsecond Network Timestamping](docs/ptp-ieee-1588-sub-microsecond-timestamping.md) | `ptp-ieee-1588-sub-microsecond-timestamping.md` |
| 188 | [QUIC & HTTP/3 Transport Protocol](docs/quic-http3-protocol.md) | `quic-http3-protocol.md` |
| 189 | [QUIC RFC 9002 Loss Detection & Congestion Control](docs/quic-loss-detection-congestion.md) | `quic-loss-detection-congestion.md` |
| 190 | [QUIC Latency Observation: The Spin Bit (RFC 9000)](docs/quic-spin-bit-latency-measurement.md) | `quic-spin-bit-latency-measurement.md` |
| 191 | [RabbitMQ Exchange Routing Patterns](docs/rabbitmq-exchange-routing.md) | `rabbitmq-exchange-routing.md` |
| 192 | [Raft Distributed Consensus Protocol](docs/raft-consensus-algorithm.md) | `raft-consensus-algorithm.md` |
| 193 | [Raft Dynamic Membership Changes & Joint Consensus](docs/raft-joint-consensus-dynamic-membership.md) | `raft-joint-consensus-dynamic-membership.md` |
| 194 | [Raft Consensus: Pre-Vote Protocol Extension](docs/raft-pre-vote-protocol-extension.md) | `raft-pre-vote-protocol-extension.md` |
| 195 | [Token Bucket & Leaky Bucket Rate Limiting](docs/rate-limiting-token-bucket.md) | `rate-limiting-token-bucket.md` |
| 196 | [RDMA & RoCEv2 Architecture for Ultra-Low Latency](docs/rdma-rocev2-infiniband-verbs.md) | `rdma-rocev2-infiniband-verbs.md` |
| 197 | [Recursive SNARKs & Nova Incrementally Verifiable Computation](docs/recursive-snark-folding-halo2-nova.md) | `recursive-snark-folding-halo2-nova.md` |
| 198 | [Redis Cluster & Hash Slot Sharding](docs/redis-cluster-hash-slots.md) | `redis-cluster-hash-slots.md` |
| 199 | [Redis In-Memory Store](docs/redis-commands.md) | `redis-commands.md` |
| 200 | [Redis Sentinel Failover & Monitoring](docs/redis-sentinel-high-availability.md) | `redis-sentinel-high-availability.md` |
| 201 | [Regular Expressions](docs/regex-cheatsheet.md) | `regex-cheatsheet.md` |
| 202 | [Chaitin-Briggs Graph Coloring Register Allocation](docs/register-allocation-graph-coloring.md) | `register-allocation-graph-coloring.md` |
| 203 | [RESTful API Architecture](docs/rest-api-best-practices.md) | `rest-api-best-practices.md` |
| 204 | [Ring Signatures & Confidential Transactions (RingCT)](docs/ring-signatures-confidential-transactions.md) | `ring-signatures-confidential-transactions.md` |
| 205 | [RocksDB Write Stall Mitigation & Compaction Tuning](docs/rocksdb-write-stall-tuning.md) | `rocksdb-write-stall-tuning.md` |
| 206 | [SAML 2.0 Single Sign-On (SSO)](docs/saml-sso-architecture.md) | `saml-sso-architecture.md` |
| 207 | [Linux seccomp User Notification Filters](docs/seccomp-user-notification-filter.md) | `seccomp-user-notification-filter.md` |
| 208 | [SPDZ Secure Multi-Party Computation (MPC)](docs/secure-multi-party-computation-spdz.md) | `secure-multi-party-computation-spdz.md` |
| 209 | [Web Security Headers](docs/security-headers.md) | `security-headers.md` |
| 210 | [Differential Power Analysis (DPA) & Masking Schemes](docs/side-channel-power-dpa-countermeasures.md) | `side-channel-power-dpa-countermeasures.md` |
| 211 | [SIMD AVX-512 Vectorization in Order Book Operations](docs/simd-avx-512-order-filtering-vectorization.md) | `simd-avx-512-order-filtering-vectorization.md` |
| 212 | [AVX-512 Vectorization Architecture](docs/simd-avx-512-vectorization.md) | `simd-avx-512-vectorization.md` |
| 213 | [ARM64 NEON SIMD Vectorization Architecture](docs/simd-neon-vectorization-arm64.md) | `simd-neon-vectorization-arm64.md` |
| 214 | [SIMD Parallelism: AVX2 and ARM NEON](docs/simd-vectorization-avx2.md) | `simd-vectorization-avx2.md` |
| 215 | [Google Spanner: TrueTime & External Consistency](docs/spanner-truetime-external-consistency.md) | `spanner-truetime-external-consistency.md` |
| 216 | [SQL Indexing & Query Optimization](docs/sql-indexing-strategies.md) | `sql-indexing-strategies.md` |
| 217 | [SQL Injection Prevention & Parameterization](docs/sql-injection-mitigation.md) | `sql-injection-mitigation.md` |
| 218 | [SSH Certificate Authority Architecture](docs/ssh-certificates-ca.md) | `ssh-certificates-ca.md` |
| 219 | [SSH Key Management](docs/ssh-keys.md) | `ssh-keys.md` |
| 220 | [strace System Call Tracing](docs/strace-system-calls.md) | `strace-system-calls.md` |
| 221 | [Subnetting and CIDR Notation](docs/subnets-cidr-notation.md) | `subnets-cidr-notation.md` |
| 222 | [systemd Resource Control Directives & Limits](docs/systemd-cgroup-resource-controllers.md) | `systemd-cgroup-resource-controllers.md` |
| 223 | [systemd journald Logging Architecture](docs/systemd-journald-logging.md) | `systemd-journald-logging.md` |
| 224 | [Systemd Service Units](docs/systemd-services.md) | `systemd-services.md` |
| 225 | [systemd Socket Activation Architecture](docs/systemd-socket-activation-architecture.md) | `systemd-socket-activation-architecture.md` |
| 226 | [Tar and Gzip Utilities](docs/tar-compression.md) | `tar-compression.md` |
| 227 | [BBR Congestion Control (Bottleneck Bandwidth and RTT)](docs/tcp-bbr-congestion-control.md) | `tcp-bbr-congestion-control.md` |
| 228 | [Solarflare ef_vi Low-Level Driver Architecture](docs/tcp-direct-ef-vi-solarflare-low-latency.md) | `tcp-direct-ef-vi-solarflare-low-latency.md` |
| 229 | [TCP/IP Protocol Suite](docs/tcp-ip-fundamentals.md) | `tcp-ip-fundamentals.md` |
| 230 | [TCP Selective Acknowledgment (SACK RFC 2018)](docs/tcp-sack-selective-acknowledgment.md) | `tcp-sack-selective-acknowledgment.md` |
| 231 | [TCP SYN Flood & SYN Cookies (net.ipv4.tcp_syncookies)](docs/tcp-syn-flood-syncookies.md) | `tcp-syn-flood-syncookies.md` |
| 232 | [TCP Sliding Window & Window Scale Option (RFC 7323)](docs/tcp-window-scaling-flow-control.md) | `tcp-window-scaling-flow-control.md` |
| 233 | [Terraform State Management & Remote Backends](docs/terraform-state-management.md) | `terraform-state-management.md` |
| 234 | [Shamir's Secret Sharing & Threshold Signatures (FROST)](docs/threshold-cryptography-shamir-secret-sharing.md) | `threshold-cryptography-shamir-secret-sharing.md` |
| 235 | [FROST: Flexible Round-Optimized Schnorr Threshold Signatures](docs/threshold-signatures-frost-protocol.md) | `threshold-signatures-frost-protocol.md` |
| 236 | [Tick-to-Trade Profiling & Latency Instrumentation](docs/tick-to-trade-latency-measurement-instrumentation.md) | `tick-to-trade-latency-measurement-instrumentation.md` |
| 237 | [TLS 1.3 0-RTT Early Data Resumption (RFC 8446)](docs/tls-1-3-0-rtt-resumption.md) | `tls-1-3-0-rtt-resumption.md` |
| 238 | [TLS Encrypted Client Hello (ECH) Architecture](docs/tls-1-3-enc-client-hello-ech.md) | `tls-1-3-enc-client-hello-ech.md` |
| 239 | [X.509 Certificate Chains & Public Key Infrastructure](docs/tls-certificate-chains-pki.md) | `tls-certificate-chains-pki.md` |
| 240 | [TLS 1.3 Cryptographic Handshake](docs/tls-ssl-handshake.md) | `tls-ssl-handshake.md` |
| 241 | [Tmux Terminal Multiplexer](docs/tmux-shortcuts.md) | `tmux-shortcuts.md` |
| 242 | [Time-Based One-Time Password (TOTP RFC 6238)](docs/totp-2fa-rfc6238.md) | `totp-2fa-rfc6238.md` |
| 243 | [TPM 2.0 Measured Boot & Remote Attestation](docs/tpm2-measured-boot-remote-attestation.md) | `tpm2-measured-boot-remote-attestation.md` |
| 244 | [Invariant TSC & Sub-Nanosecond Benchmarking](docs/tsc-clock-drift-invariant-rdtsc.md) | `tsc-clock-drift-invariant-rdtsc.md` |
| 245 | [Valgrind Memcheck & Memory Profiling](docs/valgrind-memcheck.md) | `valgrind-memcheck.md` |
| 246 | [Vector Clocks & Distributed Causal Ordering](docs/vector-clocks-causal-ordering.md) | `vector-clocks-causal-ordering.md` |
| 247 | [Vector Clocks & Causal Ordering](docs/vector-clocks-versioning.md) | `vector-clocks-versioning.md` |
| 248 | [Vector Embeddings & HNSW Graph Indexing](docs/vector-embeddings-hnsw-indexing.md) | `vector-embeddings-hnsw-indexing.md` |
| 249 | [Verifiable Delay Functions (VDF) Architecture](docs/verifiable-delay-functions-vdf.md) | `verifiable-delay-functions-vdf.md` |
| 250 | [Verifiable Delay Functions (VDF)](docs/verifiable-delay-functions-wesolowski.md) | `verifiable-delay-functions-wesolowski.md` |
| 251 | [Verifiable Random Functions (VRF)](docs/verifiable-random-functions-ecvrf.md) | `verifiable-random-functions-ecvrf.md` |
| 252 | [Write-Ahead Logging (WAL) & ARIES Recovery Protocol](docs/wal-write-ahead-logging-fsync.md) | `wal-write-ahead-logging-fsync.md` |
| 253 | [WebSocket RFC 6455 Protocol](docs/websocket-protocol.md) | `websocket-protocol.md` |
| 254 | [WireGuard Cryptokey Routing & Protocol Architecture](docs/wireguard-cryptokey-routing-protocol.md) | `wireguard-cryptokey-routing-protocol.md` |
| 255 | [WireGuard Cryptography: Noise_IKpsk2 Handshake](docs/wireguard-noise-ik-handshake.md) | `wireguard-noise-ik-handshake.md` |
| 256 | [WireGuard Protocol & Cryptokey Routing](docs/wireguard-vpn-cryptography.md) | `wireguard-vpn-cryptography.md` |
| 257 | [Zero-Copy Serialization: FlatBuffers & Cap'n Proto](docs/zero-copy-serialization-flatbuffers-capnp.md) | `zero-copy-serialization-flatbuffers-capnp.md` |
| 258 | [Zero-Knowledge Bulletproofs Range Proofs](docs/zero-knowledge-bulletproofs.md) | `zero-knowledge-bulletproofs.md` |
| 259 | [Zero-Knowledge Proofs (ZK-SNARKs & ZK-STARKs)](docs/zero-knowledge-proofs-intro.md) | `zero-knowledge-proofs-intro.md` |
| 260 | [Groth16 Zero-Knowledge SNARK Proof System](docs/zero-knowledge-snarks-groth16.md) | `zero-knowledge-snarks-groth16.md` |
| 261 | [zk-Rollups & Data Availability Architectures](docs/zk-rollup-data-availability-validium.md) | `zk-rollup-data-availability-validium.md` |
| 262 | [zk-SNARKs & Plonk Arithmetization Architecture](docs/zk-snarks-plonk-arithmetization.md) | `zk-snarks-plonk-arithmetization.md` |
| 263 | [zk-STARKs & Fast Reed-Solomon IOP of Proximity (FRI)](docs/zk-starks-fri-proof-protocol.md) | `zk-starks-fri-proof-protocol.md` |
| 264 | [zkEVM State Transition Circuit Architecture](docs/zkevm-state-transition-circuit-design.md) | `zkevm-state-transition-circuit-design.md` |

---

## License

MIT License. Designed and maintained by [Ismayil Hussain](https://github.com/isihussain).
