# Linux Networking: Epoll O(1) Architecture Internals

Unlike `select()` and `poll()` which require $O(N)$ linear scans through file descriptor sets on every call, `epoll` maintains kernel state across calls.

```mermaid
flowchart TD
    EpollFD[Epoll File Descriptor: eventpoll] --> RBTree[Red-Black Tree: All Monitored FDs]
    EpollFD --> ReadyList[Doubly-Linked Ready List: Active FDs]
    NIC_Interrupt[Packet Arrives at Socket] --> Callback[Kernel Waitqueue Callback: ep_poll_callback]
    Callback --> AddReadyList[Appends FD to Ready List: O(1)]
    epoll_wait[epoll_wait() Call] --> Deliver[Returns ONLY Ready List to Userspace: O(1)]
```

---

## 1. Complexity
- Registration (`EPOLL_CTL_ADD`): $O(\log N)$ in the RB-tree.
- Notification (`epoll_wait`): $O(K)$ where $K$ is the number of triggered events.
