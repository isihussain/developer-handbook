# PostgreSQL Declarative Table Partitioning

Range and list partitioning for multi-terabyte timeseries tables.

## Overview
Technical cheat sheet for `postgresql-partitioning-declarative` covering production deployment, configuration, and diagnostics.

## Commands and Usage
```bash
CREATE TABLE measurement (logdate date, temp int) PARTITION BY RANGE (logdate);
```
