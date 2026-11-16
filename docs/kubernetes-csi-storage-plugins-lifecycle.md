# Kubernetes Storage: CSI Volume Lifecycle (Provision, Attach, Mount)

The Container Storage Interface (CSI) decouples block and file storage providers from the in-tree Kubernetes codebase.

```mermaid
sequenceDiagram
    participant API as kube-apiserver
    participant Attacher as CSI External Attacher
    participant Controller as CSI Controller Plugin
    participant Driver as CSI Node Plugin (Kubelet)

    Note over API: PVC Created
    API->>Controller: CreateVolume()
    API->>Attacher: VolumeAttachment Created
    Attacher->>Controller: ControllerPublishVolume() (Attaches AWS EBS to EC2 Instance)
    API->>Driver: NodeStageVolume() (Formats ext4/xfs filesystem)
    API->>Driver: NodePublishVolume() (Bind-mounts volume into Pod directory)
```

---

## 1. NodeStage vs NodePublish
- **NodeStageVolume:** Executed once per node (formats block device and mounts to global staging path).
- **NodePublishVolume:** Executed per pod (bind-mounts from staging directory into container namespace).
