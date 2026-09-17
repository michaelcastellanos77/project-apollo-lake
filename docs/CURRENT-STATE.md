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
- RAM: 4 GB installed / approximately 3.7 GiB visible to Linux
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

**Current leading LLM:** Qwen2.5-1.5B-Instruct Q4_K_M  
**Final selection:** Not yet made

Qwen2.5-0.5B-Instruct remains the low-resource baseline. Qwen2.5-1.5B-Instruct is currently the leading evaluated candidate after completing CPU and Vulkan linguistic/performance testing.

Speech recognition:
Not yet selected

Text-to-speech:
Not yet selected

Memory system:
Not yet implemented

## Current Objective

Find the smallest practical LLM that provides satisfactory Apollo Lake conversational capability in English, Simplified Chinese and mixed English/Chinese interaction, while leaving sufficient system resources for the rest of the offline assistant.

After a model meets the five core linguistic/functional requirements, the next performance target is:

- **minimum 3.5 generated tokens/s**
- **minimum 30 prompt tokens/s**

These are project targets rather than claims about what the hardware must achieve. Generation tokens/s is the primary conversational metric; prompt tokens/s will be measured using controlled prompts because it depends strongly on input length/content.

LLM investigation will continue from the current 1.5B leader in both directions: lighter models will be tested for the possibility of meeting the linguistic requirements while exceeding the speed target, while selected heavier models will be tested to determine whether additional model capacity remains practical on the N3350/HD 500.

## LLM Evaluation Methodology

For each candidate, linguistic suitability is assessed before performance optimisation. The current methodology is:

1. Verify the model file and runtime can load the model locally.
2. Establish basic English and Simplified Chinese generation.
3. Test English→Chinese translation.
4. Test Chinese→English translation.
5. Test mixed English/Chinese conversational interaction.
6. Use llama.cpp conversation mode and the model's chat template for Instruct-model functional tests rather than relying on raw completion prompts.
7. Record llama.cpp prompt-processing and generation throughput. Generation tokens/s is the main conversational-performance metric; prompt tokens/s measures input processing and varies with prompt length/content.
8. Keep CPU tests at 2 threads on the N3350 unless a deliberate controlled test changes this.
9. Disable swap for controlled inference performance measurements so results are not silently affected by non-volatile memory paging.
10. Only investigate acceleration after basic linguistic suitability is established.
11. Where Vulkan is applicable, verify the Vulkan device independently, verify llama.cpp device discovery, and perform CPU/GPU comparison using the same model/configuration and controlled prompts where practical.
12. Record functional failures, factual-reliability problems, crashes and resource/compatibility issues as first-class results rather than discarding them.
13. Do not rank untested candidates numerically; mark them as pending evaluation.

## Current LLM Investigation

### Current provisional ranking

1. **Qwen2.5-1.5B-Instruct Q4_K_M — current leader**
2. **Qwen2.5-0.5B-Instruct Q4_K_M — low-resource baseline**

The ranking is empirical and provisional. It applies only to candidates actually evaluated on Apollo Lake. See `docs/LLM-RANKING.md` for the current ranking and pending candidate list.

### Evaluated heavier candidate

**Qwen2.5-1.5B-Instruct Q4_K_M** has completed the current linguistic and acceleration evaluation. It is currently the leading candidate but does not yet meet the later 3.5 t/s generation target based on observed Vulkan throughput.

### Pending candidates

The next candidates under consideration include:

- Qwen3-0.6B
- Hunyuan-0.5B-Instruct
- Chinese-Tiny-LLM (CT-LLM 0.9B)
- MiniCPM5 / MiniCPM5-1B
- ZGCM-1 1.1B
- Qwen2.5-Coder-1.5B
- DeepSeek-R1-Distill-Qwen-1.5B
- OpenCoder ~1.5B
- RWKV-6 ~1.6B
- InternLM2.5-1.8B-Chat

Candidate status does not imply final selection or a ranking position.

## Qwen2.5-0.5B-Instruct Baseline

See `experiments/008-qwen2.5-0.5b-llm-evaluation.md` for the complete experiment record.

Key result:

- CPU generation approximately **2.1–2.6 t/s** across recorded runs, with most substantive responses around 2.2–2.5 t/s.
- English and Simplified Chinese generation passed basic tests.
- Translation was functional but naturalness was weak.
- The tested mixed-language conversational task repeatedly failed.
- Vulkan was available, but the earlier full-offload attempt crashed and the limited-offload diagnostic did not establish a performance advantage.

## Qwen2.5-1.5B-Instruct Evaluation

See `experiments/009-qwen2.5-1.5b-llm-evaluation.md` for the complete experiment record.

### Environment correction

The first 1.5B download attempt occurred in a temporary Alpine tmpfs root and failed at approximately 934 MiB because the temporary `/` was only 1.8 GiB and RAM-backed. The partial file was removed. The machine was rebooted into the persistent eMMC installation and verified with:

```text
/dev/mmcblk0p3 on / type ext4 (rw,relatime)
```

The successful model download and all formal 1.5B inference results were then performed on the persistent installation.

### Controlled memory state

Before inference:

```text
Mem:  3.7G total, 151.7M used, 3.5G free, 3.4G available
Swap: 0 total, 0 used, 0 available
```

Swap was disabled with `swapoff -a` for the controlled inference measurements.

### CPU results

Observed generation:

- English: **1.5 t/s**
- Chinese: **1.4 t/s**
- English→Chinese: **1.2 t/s**
- Chinese→English: **1.2 t/s**
- Mixed-language: **1.2 t/s**, with recovery after clarification

CPU governor test:

- `schedutil`: **1.5 t/s** on the controlled English prompt
- `performance`: **1.5 t/s** on the same prompt

No generation-speed improvement was observed from changing the governor. The governor was restored to `schedutil`.

### Vulkan results

Persistent Vulkan stack:

- `llama.cpp-vulkan`
- `vulkan-loader`
- `mesa-vulkan-intel`
- `vulkan-tools`

Vulkan independently identified the Intel HD Graphics 500 (APL 2) using Mesa 26.1.6. llama.cpp exposed it as `Vulkan0`.

Unlike the earlier temporary-environment 0.5B test, Qwen2.5-1.5B successfully ran with extensive Vulkan offload (`-ngl 99`).

Observed generation:

- English: **2.4–2.5 t/s** across two runs
- Chinese: **2.4 t/s**
- English→Chinese: **2.4 t/s**
- Chinese→English: **2.5 t/s**
- Mixed-language: **2.3–2.4 t/s**

The observed Vulkan generation range is therefore approximately **2.3–2.5 t/s**, compared with approximately **1.2–1.5 t/s** on CPU for the same model family and tests.

### Linguistic assessment

- English generation: **pass**
- Simplified Chinese generation: **pass**
- English→Chinese: **partial pass** — understandable translation, but source repetition and some unnatural phrasing
- Chinese→English: **partial/pass** — generally natural, with a minor omission (`this afternoon`) in the tested prompt
- Mixed-language conversation: **partial pass** — initially translated instead of answering, but successfully recovered after explicit clarification
- Chinese factual-reliability test: **fail on the tested Three Kingdoms summary prompt** because of major factual/chronological errors despite fluent-looking Chinese

### Current interpretation

Qwen2.5-1.5B is currently the best **empirically evaluated** candidate in Apollo Lake, but it has not yet met the later performance target of 3.5 t/s generation. Its Vulkan acceleration is nonetheless a major positive result: the HD 500 provides useful inference acceleration for this model in the current llama.cpp configuration.

## GPU Acceleration

The Intel HD Graphics 500 is exposed through Linux `i915`, and Vulkan is available through Mesa on Alpine. Qwen2.5-1.5B demonstrated successful extensive Vulkan offload and approximately 2.3–2.5 t/s generation.

Future candidates will be tested for Vulkan acceleration where compatible and useful.

## Performance Targets

After linguistic suitability is established, the current project target is:

- **Generation:** ≥ 3.5 t/s
- **Prompt processing:** ≥ 30 t/s

A candidate should not be considered to meet these targets from a single unusually fast prompt. Prompt throughput in particular must be measured with controlled prompts of known length. Generation should also be confirmed across representative bilingual/conversational prompts.

## Open Questions

- Can a lighter model such as Qwen3-0.6B meet the five linguistic requirements while exceeding the 3.5 t/s generation and 30 t/s prompt targets?
- Does MiniCPM5-1B provide a better bilingual quality/speed trade-off?
- Can a heavier 1.8B-class model such as InternLM2.5-1.8B-Chat provide a meaningful quality improvement while remaining practical on the N3350/HD 500?
- Which candidate has the best RAM headroom for ASR, TTS and memory components?
- What quantisation provides the best quality/resource trade-off for the eventual selected model?
- Can any candidate sustain the target throughput for longer sessions rather than only short interactive tests?
- Which ASR system is practical?
- Which TTS system is practical?
- How much resource headroom is required for the complete voice pipeline?
