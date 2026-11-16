# Linux eBPF: XDP Driver Mode vs Native Mode Architecture

eBPF eXpress Data Path (XDP) provides bare-metal packet processing before Linux network stack allocation (sk_buff creation).

```mermaid
flowchart LR
    NIC[NIC RX Ring] --> Driver[Network Device Driver]
    Driver --> XDP_Hook{XDP Program Hook}
    XDP_Hook -->|XDP_DROP| Drop[Drop Packet: 0ns sk_buff cost]
    XDP_Hook -->|XDP_REDIRECT| Veth[Redirect to AF_XDP / Other NIC]
    XDP_Hook -->|XDP_TX| Bounce[Bounce Back to Same NIC]
    XDP_Hook -->|XDP_PASS| Stack[Allocate sk_buff & Pass to Linux Netstack]
```

---

## 1. Operating Modes Comparison

| Mode | Hook Location | Overhead | Driver Support |
| :--- | :--- | :--- | :--- |
| **Offloaded XDP** | NIC Hardware / SmartNIC | 0 CPU cycles | SmartNIC hardware required (Netronome, etc.) |
| **Driver / Native XDP** | Initial driver RX queue | Pre-sk_buff allocation | Modern NIC drivers (ixgbe, mlx5, virtio_net) |
| **Generic XDP** | Post-sk_buff allocation | Low (Software emulation) | Any network interface |

---

## 2. XDP Return Actions
- `XDP_DROP`: Drops malicious/unwanted DDoS traffic immediately at ingress.
- `XDP_TX`: Forwards packet back out the same interface (e.g. fast stateful load balancing).
- `XDP_REDIRECT`: Bypasses kernel networking and transfers raw frames into AF_XDP userspace sockets.
- `XDP_PASS`: Hands off packet to conventional TCP/IP kernel stack.
