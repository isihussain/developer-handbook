# Kubernetes Pod Lifecycle & Probes

startupProbe, livenessProbe, readinessProbe, and preStop hooks.

## Overview
Technical cheat sheet for `kubernetes-pod-lifecycle` covering production deployment, configuration, and diagnostics.

## Commands and Usage
```bash
kubectl get pod -o jsonpath='{.status.conditions}'
```
