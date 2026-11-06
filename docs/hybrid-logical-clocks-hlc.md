# Hybrid Logical Clocks (HLC) Architecture

Combines physical NTP wall clocks with Lamport logical clocks to provide monotonically increasing timestamps bound to physical time.

---

## 1. HLC Invariants

For any two events $e 	o e'$:
$$	ext{hlc}(e) < 	ext{hlc}(e')$$
$$|	ext{hlc}(e).	ext{physical} - 	ext{NTP.now}| \le \epsilon$$
CockroachDB and MongoDB utilize HLCs for distributed snapshot isolation without requiring atomic clocks.
