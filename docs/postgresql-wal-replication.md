# PostgreSQL Write-Ahead Logging (WAL) & Streaming

Physical replication, synchronous_commit, and replication slots.

## Overview
Technical cheat sheet for `postgresql-wal-replication` covering production deployment, configuration, and diagnostics.

## Commands and Usage
```bash
SELECT * FROM pg_stat_replication;
SELECT pg_current_wal_lsn();
```
