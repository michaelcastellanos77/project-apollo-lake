# Current Project State

Last updated: 2026-09-18

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

**Current provisional LLM leader:** Qwen3-0.6B Q4_K_M — **deferred / investigation flagged**  
**Final selection:** Not yet made

Qwen3-0.6B is no longer being actively investigated during the current development pass. After a clean reboot, the exact previously successful CPU command reproduced a segmentation fault, and Vulkan attempts also reproduced crashes. The root cause remains unresolved. Experiment 010 is explicitly flagged for later investigation if time permits.

Qwen3-0.6B is now the current provisional leader after initial CPU and Vulkan evaluation. It has a much smaller model footprint than Qwen2.5-1.5B and observed generation of approximately 3.6–4.0 t/s across the individual substantive tests. It has not passed every linguistic requirement and has not met the 30 t/s prompt-processing target, so this promotion is provisional and can be changed as more candidates are evaluated.

Qwen2.5-1.5B remains an important comparison/fallback candidate because it demonstrated stronger tested mixed-language recovery and substantial Vulkan acceleration, despite lower throughput.

Speech recognition:
Not yet selected

Text-to-speech:
Not yet selected

Memory system:
Not yet implemented

## Current Objective

**Immediate objective: establish the minimum-resource offline speech system that still provides usable speech recognition and text-to-speech on the target hardware.** LLM selection is temporarily paused so the project can determine how much RAM, CPU time and storage the speech subsystem requires alongside a future LLM.

The LLM objective remains: find the smallest practical LLM that provides satisfactory Apollo Lake conversational capability in English, Simplified Chinese and mixed English/Chinese interaction, while leaving sufficient system resources for the rest of the offline assistant.

After a model meets the five core linguistic/functional requirements, the next performance gate is:

- **minimum 3.5 generated tokens/s**
- **minimum 30 prompt tokens/s**

These are project targets rather than claims about what the hardware must achieve. Generation tokens/s is the primary conversational metric; prompt tokens/s will be measured using controlled prompts because it depends strongly on input length/content.

The search strategy is deliberately staged rather than simply moving to ever-larger models. Qwen3-0.6B has now been evaluated as the lightweight candidate and promoted provisionally; other candidates will still be tested before final selection.

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
14. After linguistic suitability, explicitly check the 3.5 t/s generation and 30 t/s prompt-processing performance gates using controlled workloads.

## Current Speech-System Investigation

**Status: next development target**

The speech subsystem has not yet been selected. The next engineering work will establish:

1. the minimum-resource offline speech-recognition system that is sufficiently usable;
2. the minimum-resource offline text-to-speech system that is sufficiently usable;
3. their idle and active RAM/CPU/storage requirements;
4. whether they can coexist with a future LLM within the 4 GB hardware envelope; and
5. a reproducible baseline configuration before the assistant pipeline is integrated.

LLM evaluation will resume after this speech baseline unless a concrete speech-system result requires revisiting the LLM choice sooner.

## Current LLM Investigation

### Current provisional ranking

1. **Qwen3-0.6B Q4_K_M — current provisional leader**
2. **Qwen2.5-1.5B-Instruct Q4_K_M — evaluated comparison/fallback**
3. **Qwen2.5-0.5B-Instruct Q4_K_M — low-resource baseline**

The ranking is empirical and provisional. It applies only to candidates actually evaluated on Apollo Lake. See `docs/LLM-RANKING.md` for the current ranking and pending candidate list.

### Qwen3-0.6B evaluation

See `experiments/010-qwen3-0.6b-llm-evaluation.md` for the complete experiment record.

Key results:

- Q4_K_M model file: **461.8 MiB**.
- Controlled inference used 2 CPU threads, 2048-token context, swap disabled and `--reasoning off`.
- English: **pass**, 3.8 t/s generation.
- Simplified Chinese: **pass**, 3.8 t/s generation.
- English→Chinese: **pass**, 4.0 t/s generation.
- Chinese→English: **partial pass**, 3.8 t/s generation; tense/naturalness issues remained.
- Mixed English/Chinese conversation: **fail** on the tested requirement; the model translated/rephrased instead of answering, including after clarification.
- Tested Chinese historical-summary task: **fail** for factual reliability; one uncontrolled/default-context attempt crashed and the controlled 2048-context repeat produced major factual/chronological errors.
- CPU generation across substantive tests: **3.6–4.0 t/s**.
- Observed prompt processing: approximately **8.5–12.0 t/s** on CPU, below the later 30 t/s target.
- Full Vulkan offload (`-ngl 100`) worked, with generation also approximately **3.6–4.0 t/s**.
- Vulkan `ngl` sweep: 1 layer was slower; 25 layers remained slightly slower; 50–100 layers reached approximately CPU-level generation throughput.
- No large generation-speed advantage from Vulkan was observed for this small model.
- Default model context attempted an approximately **4.38 GiB** allocation, which exceeded available physical RAM.

### Evaluated heavier candidate

**Qwen2.5-1.5B-Instruct Q4_K_M** has completed the current linguistic and acceleration evaluation. It remains an important comparison candidate.

Its observed Vulkan generation was approximately **2.3–2.5 t/s**, substantially below the Qwen3-0.6B observed range, while its mixed-language test recovered successfully after clarification.

### Pending candidates

The next candidates under consideration include:

**Lighter / ~1B:**
- Hunyuan-0.5B-Instruct
- Chinese-Tiny-LLM (CT-LLM 0.9B)
- MiniCPM5-1B
- ZGCM-1 1.1B

**~1.5–1.8B:**
- Qwen2.5-Coder-1.5B
- DeepSeek-R1-Distill-Qwen-1.5B
- OpenCoder ~1.5B
- RWKV-6 ~1.6B
- InternLM2.5-1.8B-Chat

Selected 2B+ candidates may be added later if the smaller candidates do not satisfy the project gates or if a larger model has a specific capability justification.

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

## GPU Acceleration

The Intel HD Graphics 500 is exposed through Linux `i915`, and Vulkan is available through Mesa on Alpine. Qwen2.5-1.5B demonstrated successful extensive Vulkan offload and approximately 2.3–2.5 t/s generation.

Qwen3-0.6B also runs successfully with full Vulkan offload (`-ngl 100`), but for this much smaller model the observed generation speed is approximately the same as CPU-only inference. The `ngl` sweep showed that very small partial offload can be slower, while larger offload reaches approximately CPU-level throughput.

Future candidates will be tested for Vulkan acceleration where compatible and useful.

## Performance Targets

After linguistic suitability is established, the current project target is:

- **Generation:** ≥ 3.5 t/s
- **Prompt processing:** ≥ 30 t/s

Qwen3-0.6B has exceeded the 3.5 t/s generation target in the individual tests performed so far, but it has not satisfied the five linguistic requirements and its prompt-processing measurements remain below 30 t/s. The generation result also requires confirmation across controlled repeated/sustained workloads before being treated as a robust benchmark result.

A candidate should not be considered to meet these targets from a single unusually fast prompt. Prompt throughput in particular must be measured with controlled prompts of known length. Generation should also be confirmed across representative bilingual/conversational prompts.

## Open Questions

- Can Qwen3-0.6B's mixed-language instruction following be improved through prompt/template configuration without sacrificing speed?
- Can Qwen3-0.6B provide stable long-form inference without segmentation faults under a controlled context?
- Can its generation speed remain above 3.5 t/s over sustained operation?
- Can any candidate provide stronger bilingual conversation and factual reliability while retaining a similar performance/resource envelope?
- Does MiniCPM5-1B provide a better bilingual quality/speed trade-off?
- Can a heavier 1.8B-class model such as InternLM2.5-1.8B-Chat provide a meaningful quality improvement while remaining practical on the N3350/HD 500?
- Do any ~1.5B candidates materially improve factual reliability or instruction following while remaining within the performance/resource envelope?
- Which candidate has the best RAM headroom for ASR, TTS and memory components?
- What quantisation provides the best quality/resource trade-off for the eventual selected model?
- Which ASR system is practical?
- Which TTS system is practical?
- How much resource headroom is required for the complete voice pipeline?
