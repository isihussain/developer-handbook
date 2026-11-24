# Bloom Filters & Count-Min Sketches

A Bloom filter is a compact probabilistic set-membership structure. It can say **definitely absent** or **possibly present**: false positives are possible, but a correctly maintained standard Bloom filter has no false negatives. Use it to avoid an expensive lookup when a negative answer is common; verify positive answers against the source of truth.

## Sizing a Bloom filter

For `n` expected items and target false-positive probability `p`, the approximate optimal size is:

`m = -n ln(p) / (ln 2)^2` bits

The corresponding optimal number of hash functions is:

`k = (m / n) ln 2`

For example, a 1% target needs about 9.6 bits per item and 7 hash functions. If the filter receives more items than planned, its false-positive rate rises. A standard Bloom filter does not support deletion because clearing a bit could make another inserted item appear absent; counting Bloom filters use counters to support deletion at extra memory cost.

## Where it fits

- Check a database or storage index before performing a costly point lookup.
- Filter items that are certainly not in a cache or set.
- Keep the exact dataset elsewhere; the filter is not a replacement for it.

A Bloom filter is a poor fit when false positives cause irreversible actions, or when the set changes frequently and deletion is required.

## References

- Redis, [Bloom filter sizing and false-positive rates](https://redis.io/docs/latest/develop/data-types/probabilistic/bloom-filter/).
- Burton H. Bloom, [Space/Time Trade-offs in Hash Coding with Allowable Errors](https://doi.org/10.1145/362686.362692).
