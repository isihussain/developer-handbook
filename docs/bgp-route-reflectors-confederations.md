# Scaling iBGP: Route Reflectors & AS Confederations

Eliminates the full-mesh $O(N^2)$ peering requirement in internal BGP routing domains.

---

## 1. Loop Prevention Attributes

- **ORIGINATOR_ID**: 4-byte router ID of the route creator in the local AS.
- **CLUSTER_LIST**: Sequence of Cluster IDs traversed. If a router sees its own Cluster ID, it discards the update.
