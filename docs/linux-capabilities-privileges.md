# Linux Capabilities (cap_net_bind_service)

Splitting root privileges into fine-grained capability sets.

## Overview
Technical cheat sheet for `linux-capabilities-privileges` covering production deployment, configuration, and diagnostics.

## Commands and Usage
```bash
getcap /path/to/binary
setcap 'cap_net_bind_service=+ep' /path/to/binary
```
