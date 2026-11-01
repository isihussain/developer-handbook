# SQL Injection Prevention & Parameterization

Separating SQL code from data via prepared statements.

## Overview
Technical cheat sheet for `sql-injection-mitigation` covering production deployment, configuration, and diagnostics.

## Commands and Usage
```bash
cursor.execute('SELECT * FROM users WHERE id = %s', (user_id,))
```
