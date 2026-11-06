# Linux VFS Directory Entry (dentry) Cache & Path Walk

The Virtual Filesystem (VFS) caches pathname components in memory (`dcache`) to eliminate repeated disk inode lookups during path resolution.

```mermaid
flowchart LR
    Path["/usr/bin/python3"] --> Root[Root dentry '/']
    Root --> D1["dentry 'usr' (Hash lookup)"]
    D1 --> D2["dentry 'bin' (Hash lookup)"]
    D2 --> D3["dentry 'python3' (Hash lookup)"]
    D3 --> Inode[struct inode (Metadata & Extents)]
```

---

## 1. RCU Path-Walk vs Ref-Walk

VFS implements two distinct path resolution algorithms in `fs/namei.c`:

- **RCU-walk (Fast Path)**:
  - Traverses `dentry` pointers under `rcu_read_lock()` without acquiring spinlocks or incrementing reference counts.
  - Detects concurrent renames or unmounts via sequence counters (`d_seq`). If sequence mismatch occurs, aborts and falls back to ref-walk.
- **Ref-walk (Slow Path)**:
  - Acquires reference locks (`dget`) and filesystem mutexes on each directory component.
  - Used when resolving symlinks, accessing unmounted filesystems, or encountering missing cache entries.

---

## 2. Negative Dentries

When an application queries a non-existent file (`stat("/etc/app.conf")`), VFS allocates a **Negative dentry** (`dentry->d_inode == NULL`).
Subsequent lookups return `ENOENT` in-memory without issuing block device lookups.
