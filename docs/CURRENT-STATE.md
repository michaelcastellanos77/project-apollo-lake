# Current Project State

Last updated: 2026-09-17

## Current Phase

**Phase 2 — LLM research and selection**

Alpine Linux is now installed persistently on the Lenovo's internal eMMC. SSH access from the Chromebook has been verified, so remaining configuration can be performed remotely.

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
- Vulkan: exposed through Mesa; llama.cpp detects `Vulkan0`
- Storage: 29.1 GiB internal eMMC
- Raw sequential eMMC read: 161.2 MB/s
- CPU thermal trip point: 105.05°C
- Short controlled 2-core sustained-load test: 54°C maximum observed CPU temperature

Phase 1 established the practical hardware baseline. Further hardware benchmarking will only be performed if a later engineering decision requires it.

## Operating System

**Reference operating system:** Alpine Linux x86-64

Alpine is now installed persistently on the Lenovo's internal eMMC using the `sys` installation mode. The previous Ubuntu installation was erased. Wired Ethernet and OpenSSH have been configured, and SSH access from the Chromebook has been successfully verified.

The operating system will be kept deliberately minimal. Components will be added only as required for Apollo Lake, including SSH, English/Chinese text input, LLM inference, speech recognition and text-to-speech.

The OS is not currently an optimisation target. It will only be revisited if a concrete requirement demonstrates that Alpine is unsuitable.

## AI System

LLM:
**Not yet selected.** Qwen2.5-0.5B-Instruct has now been evaluated as the first low-resource baseline and retained for comparison.

Speech recognition:
Not yet selected

Text-to-speech:
Not yet selected

Memory system:
Not yet implemented

## Current Objective

Find the smallest practical LLM that provides satisfactory Apollo Lake conversational capability in English, Simplified Chinese and mixed English/Chinese interaction, while leaving sufficient system resources for the rest of the offline assistant.

LLM investigation will proceed from very small models upward rather than attempting to maximise model size.

## LLM Evaluation Methodology

For each candidate, linguistic suitability is assessed before performance optimisation. The current methodology is:

1. Verify the model file and runtime can load the model locally.
2. Establish basic English and Simplified Chinese generation.
3. Test English→Chinese translation.
4. Test Chinese→English translation.
5. Test mixed English/Chinese conversational interaction.
6. Use llama.cpp conversation mode and the model's chat template for Instruct-model functional tests rather than relying on raw completion prompts.
7. Record llama.cpp prompt-processing and generation throughput. Generation tokens/s is the main conversational-performance metric; prompt tokens/s measures input processing and varies with prompt length/content.
8. Keep CPU tests at 2 threads on the N3350 unless a later controlled test changes this deliberately.
9. Only investigate acceleration after basic linguistic suitability is established.
10. Where Vulkan is applicable, verify the Vulkan device independently, verify llama.cpp device discovery, and perform a quick CPU/GPU comparison. GPU results are not treated as rigorous benchmarks unless workload and settings are controlled.
11. Record functional failures, crashes and resource/compatibility problems as first-class results rather than discarding them.

## Current LLM Investigation

Initial candidate models include:

- BAAI Aquila-135M-Instruct
- Google Gemma 3 270M
- Qwen2.5-0.5B-Instruct
- Qwen3-0.6B

The first heavier-model investigation will now expand to approximately 0.9B–1.8B candidates, including Qwen2.5-1.5B and other bilingual/Chinese-capable candidates. Candidate status does not imply final selection.

## Qwen2.5-0.5B-Instruct Baseline

Model evaluated: **Qwen2.5-0.5B-Instruct-GGUF, Q4_K_M**  
Model file size: **468.6 MiB**  
Runtime: **llama.cpp 0.0.9564-r0**  
CPU test: **2 threads**

Functional results:

- English generation: passed
- Simplified Chinese generation: passed in conversation mode
- English→Chinese: functional but naturalness was weak
- Chinese→English: functional but English was awkward
- Mixed English/Chinese conversation: failed the tested conversational task; the model repeatedly translated the question instead of answering it and did not reliably recover after clarification

Recorded CPU throughput:

- Chinese self-introduction: Prompt **11.4 t/s**, Generation **2.5 t/s**
- English→Chinese translation: Prompt **14.7 t/s**, Generation **2.5 t/s**
- Chinese→English translation: Prompt **6.8 t/s**, Generation **2.4 t/s**
- Mixed-language test: Prompt **6.7 t/s**, Generation **2.2 t/s**
- Repeated mixed-language test: Prompt **5.8 t/s**, Generation **2.1 t/s**
- Additional short clarification turns: Prompt **2.6–4.9 t/s**, Generation **2.3–2.4 t/s**

Overall observed CPU generation was approximately **2.1–2.6 t/s**, with most substantive responses around **2.2–2.5 t/s**. Prompt processing varied approximately **2.6–15.9 t/s** across recorded prompts and is not a single fixed model speed.

### Qwen Vulkan investigation

Installed and verified:

- `llama.cpp-vulkan`
- `vulkan-loader`
- `mesa-vulkan-intel`
- `vulkan-tools`

Persistent Alpine Vulkan verification identified:

- Intel HD Graphics 500 (APL 2)
- vendor ID `0x8086`
- device ID `0x5a85`
- Intel open-source Mesa driver
- Mesa 26.1.6

llama.cpp reported:

```text
Available devices:
  BLAS: OpenBLAS (0 MiB, 0 MiB free)
  Vulkan0: Intel(R) HD Graphics 500 (APL 2) (1875 MiB, 1688 MiB free)
```

A full/extensive offload attempt using `-ngl 99` crashed with a segmentation fault, so no throughput result was recorded.

A partial offload using `-ngl 1` completed successfully with a quick test:

- Prompt: **6.5 t/s**
- Generation: **2.2 t/s**

This was not a controlled CPU-vs-GPU benchmark because the prompt differed from the CPU functional tests and only one layer was offloaded. It therefore does not establish a definitive GPU speed advantage. It does establish that limited Vulkan offloading can execute while extensive offloading crashed in this configuration.

**Selection status:** Qwen2.5-0.5B-Instruct is retained as a low-resource baseline but is not currently selected as the final Apollo Lake model because mixed-language conversational instruction following and translation naturalness were below the desired target.

## GPU Acceleration

The Intel HD Graphics 500 is exposed through Linux `i915`, and Vulkan is available through Mesa on Alpine. Actual llama.cpp Vulkan device discovery has now been verified. Useful LLM acceleration is not assumed: the first Qwen full-offload attempt crashed, while limited offload completed without demonstrating a clear performance benefit.

Future candidates will be tested for Vulkan acceleration only where useful and compatible.

## Open Questions

- Which small LLM provides satisfactory English generation?
- Which small LLM provides satisfactory Simplified Chinese generation?
- Which model handles English ↔ Chinese interaction and mixed-language conversation reliably?
- What RAM does each candidate actually consume on the Lenovo?
- What generation speed and response latency are achievable?
- Which quantisation provides an appropriate quality/resource trade-off?
- Does the Intel HD Graphics 500 provide useful LLM acceleration through Vulkan for heavier candidates?
- Which inference runtime is most appropriate?
- Which ASR system is practical?
- Which TTS system is practical?
- How much resource headroom is required for memory and the complete voice pipeline?
