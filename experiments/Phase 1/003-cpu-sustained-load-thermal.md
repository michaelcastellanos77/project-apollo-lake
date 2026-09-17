# Experiment 003 — CPU Sustained Load and Thermal Behaviour

**Date:** 2026-09-15  
**Hardware:** Lenovo IdeaPad 120S-11IAP  
**CPU:** Intel Celeron N3350  
**OS:** Alpine Linux 3.24.1  
**Kernel:** 6.18.35-0-lts  
**Power:** AC connected

## Main Question

How does the N3350 behave under sustained CPU load, and what thermal conditions accompany that behaviour?
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


## Method Summary

The experiment was conducted on the Lenovo using Alpine 3.24.1.

The laptop was connected to AC power and placed on a flat hard surface with its ventilation openings unobstructed.

Before the actual benchmark:

CPU frequency reporting was verified through sysfs
CPU utilisation measurement was verified with mpstat
temperature measurement was verified through the TCPU thermal zone
stress-ng was installed and tested
the benchmark script was developed to collect the measurements into CSV format

The network was disconnected during the final benchmark run.

The script recorded, once per second:

CPU 0 utilisation
CPU 1 utilisation
CPU 0 reported frequency
CPU 1 reported frequency
TCPU temperature

The test sequence was:

30 s   idle baseline
60 s   one-core stress-ng load
30 s   idle recovery
120 s  two-core stress-ng load
30 s   final idle

During development, an error was found in the mpstat parsing logic: the script initially used the wrong output column when calculating idle percentage, resulting in incorrect utilisation values. The parser was corrected to use the proper %idle field, and a short dry run was then performed to validate the corrected instrumentation before the full experiment.

The final result set was saved locally on the Lenovo and then copied to the Chromebook. The clean CSV contained 265 measurement samples plus its header.

Results

The final experiment established approximately:

100% utilisation on the loaded core during one-core stress
~100% utilisation on both cores during two-core stress
average two-core temperature of 51.3°C
maximum observed TCPU temperature of 54°C
stable reported CPU frequency during one-core stress
no obvious thermal-frequency collapse during the short test
two-core stress-ng throughput of approximately 1.91× the one-core result


# Conclusion


The N3350 can sustain full utilisation of both cores for the duration tested without approaching its observed 105.05°C thermal trip point or showing obvious thermal-frequency collapse.

This does not establish indefinite or 30-minute endurance; that belongs to the integrated assistant stage.




## Status

Status: Completed

Controls:

AC connected
network disconnected
flat hard surface
vents unobstructed
no desktop
Alpine

Phases:

30 s idle
60 s 1-core
30 s recovery
120 s 2-core
30 s final idle

Results:

Phase	CPU0	CPU1	Avg Temp
Idle	0.13%	0.33%	35.3°C
1-core	100.00%	0.13%	44.6°C
Recovery	0.27%	0.24%	37.9°C
2-core	99.90%	99.89%	51.3°C
Final idle	0.23%	0.20%	40.3°C

1-core stress: 827.23 bogo ops/s
2-core stress: 1578.80 bogo ops/s
scaling: ~1.91×
max observed TCPU: 54°C
trip point: 105.05°C



This was a short controlled thermal/performance test and does not establish 30-minute or indefinite endurance.
The initial measurement parser used the wrong mpstat idle column and was corrected before the final run.
