## Date: 2026-09-15
## Status: Completed

## Main Question

How much of the Lenovo's physical RAM is actually visible and available to the operating system in the minimal benchmark environment?


## Method Summary

The measurement was conducted in Alpine using the minimal command-line environment.

free -h was used to obtain the human-readable memory summary.

An attempt was made to use:

swapon --show

but Alpine's BusyBox implementation did not support that form of the command.

Rather than installing additional tooling unnecessarily, the kernel's /proc/meminfo interface was used as the authoritative source for:

MemTotal
MemAvailable
SwapTotal
SwapFree

The measurements were written to a dedicated result file and copied to the Chromebook.

RAM bandwidth was intentionally not separately benchmarked because the experiment was designed to establish capacity and availability, while practical memory effects will be measured during later real LLM inference benchmarks.

## Results


RAM specifications:

4 GB physical RAM
3,841,504 kB MemTotal
3,148,180 kB MemAvailable
swap 0
minimal Alpine environment
approximately 3.0 GiB available

Memory capacity is expected to be a major constraint on local LLM size and on simultaneous ASR + LLM + TTS execution.
RAM bandwidth was not separately benchmarked because Phase 1 was intentionally limited to measurements required to establish the major hardware constraints. Actual LLM inference benchmarking will capture practical performance effects of memory subsystem behaviour.


## Conclusion

The machine has 4 GB of physical RAM, with approximately 3.66 GiB exposed to Linux. RAM capacity is expected to be a major constraint for local inference and simultaneous AI workloads.
