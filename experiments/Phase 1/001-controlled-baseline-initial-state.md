# Experiment 001 — Controlled Hardware Baseline: Initial State

**Date:** 2026-09-14
**Phase:** Phase 1 — Hardware Performance Baseline
**Status:** Initial-state snapshot complete

## Main question

What was the state of the Lenovo immediately before controlled investigation began?

## Purpose

Record the state of the Lenovo IdeaPad 120S-11IAP immediately before controlled hardware performance benchmarking. These measurements are a starting-state snapshot, not yet the performance benchmark itself.


## Method

The Lenovo IdeaPad 120S-11IAP was examined using its normal Ubuntu 26.04 LTS installation. The machine remained connected to AC power.
For subsequent benchmark runs, AC power should remain connected so that power-management behaviour is as consistent as possible.
The initial environment was intentionally left unchanged because the purpose of the experiment was to capture a starting-state snapshot, not to benchmark performance.

The following commands were used:

uname -a
lscpu
free -h
lsblk
uptime

These were used to establish:

OS and kernel
CPU architecture, cores, threads, frequencies and instruction-set information
memory capacity and current usage
storage topology and partitioning
system activity/load at the time of measurement

## Results

### Operating system / kernel

- Kernel: Linux 7.0.0-28-generic
- Architecture: x86_64
- OS: Ubuntu 26.04 LTS

### CPU

- Intel Celeron N3350 @ 1.10 GHz
- 2 physical cores
- 1 thread per core / 2 logical CPUs
- Reported maximum frequency: 2.40 GHz
- Reported minimum frequency: 800 MHz
- Current CPU scaling percentage reported by `lscpu`: 55%
- L1 data cache: 48 KiB total (2 instances)
- L1 instruction cache: 64 KiB total (2 instances)
- L2 cache: 2 MiB total (2 instances)
- Instruction-set flags include SSE/SSE2/SSSE3/SSE4.1/SSE4.2, AES and SHA-NI; AVX/AVX2 are not reported.
- VT-x is available.

### Memory

- Total memory visible to Linux: 3.3 GiB
- Used: 498 MiB
- Free: 2.6 GiB
- Available: 2.9 GiB
- Swap: 2.5 GiB total, 0 B used

### Storage

- Internal SanDisk iNAND eMMC: 29.1 GiB visible to Linux
- Device: `mmcblk1`
- EFI partition: 1 GiB
- `/boot`: 2 GiB
- LVM partition: 26.1 GiB
- Root logical volume: 13 GiB

### System activity

- Uptime at measurement: 5 minutes
- Load average: 0.34 (1 min), 0.50 (5 min), 0.27 (15 min)
- Two logical CPUs are online.


## Conclusion

The machine is lightly loaded but not completely idle. Linux has approximately 2.9 GiB of immediately available RAM and no swap is currently in use. The CPU is capable of dynamically changing frequency between 800 MHz and 2.40 GHz; the `55%` value reported by `lscpu` should not be treated as an exact current clock frequency.

The storage device is the internal eMMC, and the system uses LVM. No system configuration was changed as part of this snapshot.

These results establish the conditions from which the controlled benchmark will proceed. Actual CPU, memory, storage, thermal and graphics measurements are recorded in later experiments.


