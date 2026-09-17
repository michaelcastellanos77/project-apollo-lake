# Current Project State

Last updated: 2026-09-17

## Current Phase

**Phase 2 — LLM research and selection**

Alpine Linux has now been selected as the lightweight reference operating system for Phase 2 and subsequent development. The Lenovo will be reformatted and Alpine installed to the internal eMMC before continuing with LLM testing.

The operating-system question is temporarily considered settled. It will only be revisited if a concrete compatibility or engineering requirement shows that Alpine is unsuitable.

## Project Phase Progress

- [x] Phase 0 — Project start and hardware identification
- [x] Phase 1 — Hardware performance characterisation
- [ ] Phase 2 — LLM research and selection
- [ ] Phase 3 — Operating system and inference runtime
- [ ] Phase 4 — Speech, memory and assistant pipeline
- [ ] Phase 5 — Final optimisation
- [ ] Phase 6 — Project completion and evaluation

## Hardware

Hardware summary:

- CPU: Intel Celeron N3350, 2 cores / 2 threads
- RAM: 4 GB installed / approximately 3.66 GiB visible to Linux
- ISA: x86-64; no AVX/AVX2 reported
- iGPU: Intel HD Graphics 500 / i915
- Vulkan: exposed through Mesa
- Storage: 29.1 GiB internal eMMC
- Raw sequential eMMC read: 161.2 MB/s
- CPU thermal trip point: 105.05°C
- Short controlled 2-core sustained-load test: 54°C maximum observed CPU temperature

Phase 1 established the practical hardware baseline. Further hardware benchmarking will only be performed if a later engineering decision requires it.

## Operating System

**Reference operating system:** Alpine Linux x86-64

Alpine has been selected as the lightweight reference environment because it provides a very small base system while still providing the software ecosystem required for Apollo Lake. The project has already successfully booted Alpine on this exact Lenovo and verified key capabilities including SSH, CPU monitoring/frequency control, i915 graphics exposure, Mesa Vulkan support and low baseline RAM usage.

The Lenovo will now be reformatted before Alpine is installed to the internal eMMC. The new environment will be a persistent installation rather than the temporary removable-media environment used during Phase 1.

The operating system will be kept deliberately minimal. Components will be added only as required for Apollo Lake, including SSH, English/Chinese text input, LLM inference, speech recognition and text-to-speech.

The OS is not currently an optimisation target. It will only be revisited if a concrete requirement demonstrates that Alpine is unsuitable.

## AI System

LLM:
Not yet selected. Initial investigation is focused on extremely small bilingual models capable of generating both English and Simplified Chinese.

Speech recognition:
Not yet selected

Text-to-speech:
Not yet selected

Memory system:
Not yet implemented

## Current Objective

Find the smallest practical LLM that provides satisfactory Apollo Lake conversational capability in English, Simplified Chinese and mixed English/Chinese interaction, while leaving sufficient system resources for the rest of the offline assistant.

LLM investigation will proceed from very small models upward rather than attempting to maximise model size.

## Current LLM Investigation

Initial candidate models include:

- BAAI Aquila-135M-Instruct
- Google Gemma 3 270M
- Qwen2.5-0.5B-Instruct
- Qwen3-0.6B

These are candidates for investigation, not yet selected models.

## GPU Acceleration

The Intel HD Graphics 500 is exposed through Linux `i915`, and Vulkan is available through Mesa on Alpine. This establishes the possibility of using the iGPU for compatible workloads, but does not establish that Vulkan offloading will improve LLM performance.

During LLM testing, CPU-only inference and relevant Vulkan GPU offloading will be compared using controlled workloads where practical. The iGPU will not be assumed to provide a performance benefit merely because Vulkan is available.

## Open Questions

- Which small LLM provides satisfactory English generation?
- Which small LLM provides satisfactory Simplified Chinese generation?
- Which model handles English ↔ Chinese interaction and mixed-language conversation reliably?
- What RAM does each candidate actually consume on the Lenovo?
- What generation speed and response latency are achievable?
- Which quantisation provides an appropriate quality/resource trade-off?
- Does the Intel HD Graphics 500 provide useful LLM acceleration through Vulkan?
- Which inference runtime is most appropriate?
- Which ASR system is practical?
- Which TTS system is practical?
- How much resource headroom is required for memory and the complete voice pipeline?
