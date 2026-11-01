# Path MTU Discovery & TCP MSS Clamping

ICMP fragmentation needed packets and MSS clamping at router egress.

## Overview
Technical cheat sheet for `mtu-mss-path-discovery` covering production deployment, configuration, and diagnostics.

## Commands and Usage
```bash
ping -s 1472 -D 1.1.1.1
iptables -t mangle -A POSTROUTING -p tcp --tcp-flags SYN,RST SYN -j TCPMSS --clamp-mss-to-pmtu
```
