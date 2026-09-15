# Experiment 003 — CPU Sustained Load and Thermal Behaviour

**Date:** 2026-09-15  
**Hardware:** Lenovo IdeaPad 120S-11IAP  
**CPU:** Intel Celeron N3350  
**OS:** Alpine Linux 3.24.1  
**Kernel:** 6.18.35-0-lts  
**Power:** AC connected

## Question

How does the Intel Celeron N3350 behave under sustained CPU workloads, and what level of CPU performance can it maintain without excessive thermal throttling?

## Purpose

This experiment establishes a practical CPU performance and thermal baseline for Project Apollo Lake.

The results will contribute to the hardware ceiling: the maximum practically sustainable CPU performance available to the project before considering additional operating-system or AI-software overhead.

## Measurements

The experiment will record:

- CPU utilisation
- Reported operating frequency of CPU 0
- Reported operating frequency of CPU 1
- CPU temperature
- Sustained-load behaviour over time
- Stress-ng performance metrics

## Measurement sources

CPU utilisation:

    mpstat

CPU frequency:

    /sys/devices/system/cpu/cpu0/cpufreq/scaling_cur_freq
    /sys/devices/system/cpu/cpu1/cpufreq/scaling_cur_freq

CPU temperature:

    /sys/class/thermal/thermal_zone4/temp

CPU workload:

    stress-ng

## Initial Instrument Check

Before the experiment:

- CPU 0 reported frequency: 795935 kHz (~796 MHz)
- CPU 1 reported frequency: 1474343 kHz (~1.474 GHz)
- TCPU temperature: 35000 millidegrees C (35°C)

The frequency values demonstrate that the kernel exposes per-CPU reported frequency values and that the two cores may operate at different frequencies when lightly loaded.

## Planned Tests

### Test 1 — Idle baseline

Record CPU utilisation, frequency and temperature before applying sustained CPU load.

### Test 2 — One-core sustained load

Apply sustained CPU load to one worker.

Measure:

- CPU utilisation
- frequency
- temperature
- stress-ng performance

### Test 3 — Two-core sustained load

Apply sustained CPU load to two workers, corresponding to the N3350's two physical CPU cores.

Measure:

- CPU utilisation
- frequency
- temperature
- stress-ng performance

### Test 4 — Sustained behaviour

Analyse whether frequency and performance remain stable during prolonged two-core load.

Look for evidence of:

- thermal throttling
- frequency reduction
- performance degradation
- temperature stabilisation

## Experimental Controls

- Laptop remains connected to AC power.
- Network will be disconnected during the actual benchmark.
- No desktop environment will be running.
- Benchmark results will be saved locally.
- The temporary Alpine environment is used as a controlled benchmark environment and is not yet the selected final Apollo Lake operating system.
- The experiment will use conservative thermal limits and will not intentionally drive the CPU toward its 105°C thermal trip point.

## Hypothesis

The N3350 will reach high CPU utilisation under sustained load and will initially operate at relatively high reported frequencies, followed by frequency and temperature stabilisation as the CPU reaches its thermal and power operating limits.

The exact sustained performance and thermal behaviour must be measured rather than assumed.

## Status

Experiment design prepared. Benchmark execution has not yet begun.
