# Current Project State

Last updated: 2026-09-17

## Current Phase

**Phase 2 — LLM research and selection**

The project is currently staying on Ubuntu 26.04 while small bilingual LLMs are investigated. The final operating system has deliberately not been selected yet.

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

**Current test environment:** Ubuntu 26.04 LTS

The final operating system has not been selected.

The current strategy is to test LLMs on Ubuntu first. If Ubuntu prevents a suitable LLM from running satisfactorily, a lighter operating system will be investigated. Even if a suitable LLM is found on Ubuntu, a lighter operating system may later be evaluated to free resources for speech recognition, text-to-speech, memory and other assistant components.

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

## Open Questions

- Which small LLM provides satisfactory English generation?
- Which small LLM provides satisfactory Simplified Chinese generation?
- Which model handles English ↔ Chinese interaction and mixed-language conversation reliably?
- What RAM does each candidate actually consume on the Lenovo?
- What generation speed and response latency are achievable?
- Which quantisation provides an appropriate quality/resource trade-off?
- Does the selected LLM require a lighter operating system?
- Which inference runtime is most appropriate?
- Which ASR system is practical?
- Which TTS system is practical?
- How much resource headroom is required for memory and the complete voice pipeline?
