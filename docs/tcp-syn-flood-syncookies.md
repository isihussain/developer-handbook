# TCP SYN Flood & SYN Cookies (net.ipv4.tcp_syncookies)

Cryptographic timestamp encoding in initial sequence numbers.

## Overview
Technical cheat sheet for `tcp-syn-flood-syncookies` covering production deployment, configuration, and diagnostics.

## Commands and Usage
```bash
sysctl -w net.ipv4.tcp_syncookies=1
netstat -s | grep -i listen
```
