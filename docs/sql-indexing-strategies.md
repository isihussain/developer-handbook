# SQL Indexing & Query Optimization

B-tree, Hash, GIN, GiST indexes, composite index ordering, and EXPLAIN ANALYZE.

## Overview
Technical cheat sheet for `sql-indexing-strategies` covering production deployment, configuration, and diagnostics.

## Commands and Usage
```bash
EXPLAIN ANALYZE SELECT * FROM orders WHERE customer_id = 42;
```
