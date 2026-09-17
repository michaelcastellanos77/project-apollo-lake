# Experiment 007 — Persistent Alpine Installation

**Date:** 2026-09-17
**Status:** Complete

## Objective

Replace the previous Ubuntu installation with a persistent Alpine Linux installation on the Lenovo IdeaPad 120S-11IAP internal eMMC, then establish SSH access from the Chromebook.

## Installation

- Alpine Linux: 3.24
- Installation mode: `sys` (persistent system installation)
- Target: internal eMMC
- Ubuntu installation and its previous partition/LVM layout were erased during installation.
- External Alpine installation media was removed before the first boot of the installed system.

## Network and Remote Access

- Wired Ethernet interface: `eth0`
- IPv4: DHCP
- IPv6: not configured
- SSH server: OpenSSH
- SSH service: running
- Normal user: `apollo`
- SSH login from the Chromebook: verified successful

The initial SSH connection from the Chromebook was blocked by the expected SSH host-key mismatch after reinstalling the Lenovo. The old host key was removed from the Chromebook's `known_hosts`, after which SSH login succeeded.

## Post-install Baseline

### Kernel

```text
Linux apollo-lake 6.18.35-0-lts #1-Alpine SMP PREEMPT_DYNAMIC 2026-06-09 12:34:47 x86_64 Linux
```

### Memory

```text
              total        used        free      shared  buff/cache   available
Mem:           3.7G      150.1M        3.1G       43.1M      413.5M        3.3G
Swap:             0           0           0
```

At this baseline the installed Alpine system exposes approximately 3.7 GiB RAM, with approximately 3.3 GiB available and no swap in use.

### Storage

`lsblk` was not installed in the minimal persistent Alpine environment at this point, so the post-install block-device layout was not recorded with `lsblk` during this checkpoint.

## Result

The Lenovo is now running Alpine Linux persistently from its internal eMMC, with wired networking and working SSH access from the Chromebook. The system is ready for minimal package installation and LLM investigation.
