# Hardware

## Device

**Lenovo IdeaPad 120S-11IAP**

## CPU

**Intel Celeron N3350**

- Architecture: x86-64
- Cores: 2
- Threads: 2
- Base frequency: 1.10 GHz
- Maximum reported frequency: 2.40 GHz
- Minimum reported frequency: 800 MHz
- L2 cache: 2 MiB
- Socket count: 1
- CPU family: 6
- Model: 92
- Stepping: 9
- Virtualisation: Intel VT-x

### CPU Instruction Sets

The CPU reports support for a number of instruction sets including:

- SSE
- SSE2
- SSSE3
- SSE4.1
- SSE4.2
- AES
- SHA-NI
- AVX is not reported in the CPU flags.

The absence of AVX/AVX2 is potentially significant for local AI inference performance and will be investigated during benchmarking.

## Memory

BIOS-reported memory:

**4096 MB**

Linux-reported memory:

**3.3 GiB**

At the time of the initial hardware investigation:

- Used: 496 MiB
- Free: 2.6 GiB
- Available: 2.9 GiB
- Swap: 2.5 GiB
- Swap used: 0 B

## Storage

Physical storage:

**SanDisk iNAND 32 GB eMMC**

Linux reports:

**29.1 GiB**

Partition layout:

- EFI partition: approximately 1 GiB
- `/boot`: approximately 2 GiB
- LVM partition: approximately 26.1 GiB

The root logical volume is currently approximately 13 GiB.

At the time of the initial investigation:

- Root filesystem size: 13 GiB
- Used: 9.5 GiB
- Available: 2.7 GiB
- Usage: 78%

The LVM configuration will be investigated to determine whether additional storage can be allocated to the root filesystem.

## Operating System

Linux kernel:

`7.0.0-28-generic`

Distribution:

Ubuntu 26.04 LTS

The final operating system configuration has not yet been selected.

## Initial Assessment

The system is extremely resource constrained.

The principal expected bottlenecks are:

1. CPU compute performance
2. Limited physical RAM
3. Lack of AVX/AVX2
4. Slow eMMC storage
5. Very limited available disk space

These constraints make hardware-aware model selection, quantisation and system optimisation central to the project.
