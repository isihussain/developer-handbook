# DNSSEC NSEC3 Authenticated Denial of Existence (RFC 5155)

Prevents DNS zone walking (enumerating all domain records in a DNSSEC-signed zone) using salted iterative hashing.

---

## 1. Hash Calculation

$$	ext{Hash} = H^k(	ext{OwnerName} \parallel 	ext{Salt})$$
- Returns proof that no record exists between hashed boundaries $H_A$ and $H_B$.
