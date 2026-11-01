# Redis Cluster & Hash Slot Sharding

16,384 hash slots, CRC16 hashing, and MOVED/ASK redirects.

## Overview
Technical cheat sheet for `redis-cluster-hash-slots` covering production deployment, configuration, and diagnostics.

## Commands and Usage
```bash
redis-cli --cluster check 127.0.0.1:7000
```
