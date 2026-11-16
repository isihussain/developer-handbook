# Container Internals: containerd Content Store Architecture

containerd stores immutable OCI image blobs in a content-addressable storage engine indexed by SHA-256 digests.

```mermaid
flowchart TD
    PullImage["ctr image pull quay.io/app:v1"] --> Fetch[Fetch OCI Layer Blobs]
    Fetch --> ContentStore["Content Store: /var/lib/containerd/io.containerd.content.v1.content/"]
    ContentStore --> Ingest["Ingest Directory: Atomic CAS rename upon verification"]
    ContentStore --> BoltDB["Metadata: /var/lib/containerd/io.containerd.metadata.v1.bolt/meta.db"]
    BoltDB --> Snapshotter[Snapshotter: Overlayfs mounts layers into rootfs]
```

---

## 1. Content Addressability
Every layer is identified strictly by its cryptographic hash (`sha256:...`), guaranteeing deduplication across distinct container images sharing common base layers.
