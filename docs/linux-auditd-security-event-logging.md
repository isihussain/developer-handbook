# Linux Audit Subsystem (auditd) & Kernel Syscall Rules

The Linux Audit framework intercepts system calls and filesystem accesses to generate tamper-evident security audit trails.

---

## 1. Rule Syntax & Filters

```bash
# Monitor execution of sensitive binaries
auditctl -w /usr/bin/sudo -p x -k privilege_escalation

# Audit file attribute changes in /etc
auditctl -w /etc/shadow -p wa -k shadow_modification

# Audit all setuid / setgid syscall invocations by unprivileged users
auditctl -a always,exit -F arch=b64 -S setuid,setgid,setreuid -F auid>=1000 -k credential_change
```

Rules set with `-e 2` enforce kernel immutability: audit rules cannot be altered or cleared without a complete system reboot.
