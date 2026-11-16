# Full-Text Search: Apache Lucene Inverted Index and Finite State Transducers

Lucene (powering Elasticsearch and OpenSearch) maps terms to posting lists using memory-efficient Finite State Transducers (FST).

```mermaid
flowchart LR
    SearchTerm["Term: 'distributed'"] --> FST[FST Term Dictionary: Compact directed acyclic graph in RAM]
    FST --> Offset[File Offset in .tim file]
    Offset --> Postings[Posting List in .doc: Roaring Bitmap of Document IDs]
    Postings --> ResultDocs[Document IDs [12, 45, 992]]
```

---

## 1. Roaring Bitmap Compression
Posting lists are compressed using Roaring Bitmaps: dynamically choosing between array containers (sparse) and bitset containers (dense) to execute fast SIMD bitwise intersections.
