# DNSSEC Cryptographic Validation

DNSSEC lets a validating resolver authenticate DNS data and detect modification. It adds signatures and a chain of trust; it does not encrypt DNS queries or prove that a domain's content is safe.

A resolver starts from a configured trust anchor, normally the root key, and follows delegations: a parent zone's signed DS record authenticates a child DNSKEY, which in turn verifies the child's RRSIG records. The resolver then checks the signature over the requested RRset and its validity period.

```bash
# Ask delv to validate the answer it receives
 delv @8.8.8.8 example.com A
```

`delv` performs DNSSEC validation using its trust anchors. A result can be **secure** (validated), **insecure** (a delegation is unsigned), or **bogus** (validation was expected but failed). A timeout or `SERVFAIL` alone does not identify which validation step failed; inspect the resolver's diagnostics and confirm the zone's DS/DNSKEY chain.

## Records to recognize

- **DNSKEY:** public key material published by a zone.
- **DS:** digest of a child DNSKEY, published in the parent zone.
- **RRSIG:** signature covering an RRset, with inception and expiration times.
- **NSEC/NSEC3:** signed proofs used to show that a name or record type does not exist.

## References

- ISC BIND, [delv manual](https://bind9.readthedocs.io/en/latest/manpages.html#delv-dnssec-lookaside-validation-tool).
- IETF, [RFC 4033: DNS Security Introduction and Requirements](https://datatracker.ietf.org/doc/html/rfc4033).
