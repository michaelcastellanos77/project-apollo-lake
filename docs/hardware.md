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


### Operating System, Storage and System Management

Further investigation of the system software, storage configuration, CPU frequency management and thermal state produced the following findings:

- **Operating system:** The laptop is running Ubuntu 26.04 LTS (codename `resolute`).
- **Storage management:** The internal 32 GB eMMC storage uses LVM. The LVM volume group contains approximately 26.1 GB, with approximately 13.0 GB currently allocated to the root logical volume and approximately 13.0 GB remaining unallocated within the volume group.
- **CPU frequency management:** The CPU currently uses the Linux `schedutil` frequency governor, which dynamically adjusts CPU frequency according to system workload.
- **Thermal state:** At the time of measurement, the reported Linux thermal-zone readings ranged from approximately 20°C to 38°C. These readings represent multiple thermal zones and should not yet be assumed to correspond directly to CPU temperature.
- **Initial implication:** The machine has substantial unallocated capacity within its LVM volume group. CPU frequency is being managed dynamically by Linux, and the system was not reporting high thermal readings during this measurement.

**Investigation method:** The following Linux commands were used to obtain these findings: `lsb_release -a`, `sudo vgs`, `sudo lvs`, the CPU frequency governor query under `/sys/devices/system/cpu/`, and the thermal-zone query under `/sys/class/thermal/`.
