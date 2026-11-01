# Kubernetes Services & kube-proxy (iptables vs IPVS)

ClusterIP, NodePort, LoadBalancer, and connection tracking.

## Overview
Technical cheat sheet for `kubernetes-services-kube-proxy` covering production deployment, configuration, and diagnostics.

## Commands and Usage
```bash
iptables-save | grep KUBE-SERVICES
ipvsadm -ln
```
