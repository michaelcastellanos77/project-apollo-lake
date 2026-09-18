# Apollo Lake LLM Ranking

**Last updated:** 2026-09-18  
**Phase:** Phase 2 — LLM research and selection

## Purpose

This is a **provisional engineering ranking**, based only on models actually evaluated on the Apollo Lake target hardware. It is not a ranking of models in general.

The selection sequence is:

1. Satisfy the five core linguistic/functional requirements:
   - coherent English
   - coherent Simplified Chinese
   - English→Chinese translation
   - Chinese→English translation
   - mixed English/Chinese conversation
2. Among models that meet those requirements sufficiently, target:
   - **minimum 3.5 t/s generation**
   - **minimum 30 t/s prompt processing**
3. Consider RAM headroom, stability, Vulkan compatibility, quantisation and suitability for the complete voice-assistant pipeline.

A model that has not been tested is **not ranked below an evaluated model merely because it is smaller/larger or appears weaker on paper**. It is marked `Pending evaluation`.

## Current provisional ranking

| Rank | Model | Parameters | Current status | Observed generation | Key evidence |
|---|---|---:|---|---:|---|
| **1** | **Qwen3-0.6B Q4_K_M** | 0.6B | **Historical provisional leader; 🚩 deferred investigation** | **3.6–4.0 t/s in original runs** | Original experiment showed strong throughput, but post-reboot reproduction now segfaults on the previously successful CPU command; Vulkan attempts also crash |
| **2** | **Qwen2.5-1.5B-Instruct Q4_K_M** | 1.5B | Evaluated comparison candidate | **2.3–2.5 t/s Vulkan** | Stronger tested mixed-language recovery; useful Vulkan acceleration; lower observed generation throughput |
| **3** | **Qwen2.5-0.5B-Instruct Q4_K_M** | 0.5B | Evaluated low-resource baseline | **2.1–2.6 t/s CPU** | Runs reliably; weaker translation naturalness and failed tested mixed-language interaction |

**Important:** the ranking is provisional and can change as more candidates are evaluated. **As of 2026-09-18, LLM evaluation is paused while the speech subsystem is established. Qwen3-0.6B is explicitly deferred for later investigation.** Qwen3-0.6B is promoted because its observed generation throughput is substantially higher than Qwen2.5-1.5B while using a much smaller model footprint. It has **not** passed every linguistic requirement and has **not** met the 30 t/s prompt-processing target, so this is not a final model selection.

## Performance gate

Once a model passes the five linguistic/functional requirements, Apollo Lake applies the following performance target:

- **Generation:** ≥ 3.5 t/s
- **Prompt processing:** ≥ 30 t/s

A candidate should not be considered to meet these targets from a single unusually fast prompt. Prompt throughput must be measured using controlled prompts of known length/content, and generation should be confirmed across representative bilingual/conversational prompts.

Qwen3-0.6B has produced **3.6–4.0 t/s generation** across the individual substantive CPU and Vulkan tests performed so far, but this is not yet a statistically rigorous or sustained benchmark result.

## 🚩 Qwen3-0.6B — deferred investigation

Experiment 010 is intentionally flagged for later investigation rather than being allowed to block the project.

After a clean reboot and with swap disabled, the exact CPU command that had previously produced successful Qwen3 results reproduced a **segmentation fault**. The model also reproduced segmentation faults during Vulkan/iGPU attempts. The root cause has not been isolated.

This does **not** establish that Alpine, llama.cpp, or the Intel HD Graphics 500 Vulkan stack is generally broken: Qwen2.5-1.5B subsequently ran successfully on both CPU and Vulkan, with approximately **1.7 t/s CPU** and **2.5 t/s Vulkan** generation on the same English benchmark prompt.

Qwen2.5-0.5B was also re-downloaded and ran on CPU at **2.2 t/s**, while its full Vulkan attempt segfaulted.

Return to Experiment 010 if time permits. Possible future investigation areas include the Qwen3 GGUF/runtime interaction and the Intel Vulkan-driver path, but no cause should be assumed until tested.

## Qwen3-0.6B current position

Qwen3-0.6B Q4_K_M is currently first because it offers the best observed combination of small model footprint and generation throughput among the candidates evaluated so far. This promotion is explicitly provisional.

Observed strengths:

- Q4_K_M model file is only **461.8 MiB**.
- Basic English generation passed.
- Basic Simplified Chinese generation passed.
- English→Chinese translation passed the tested task.
- Observed generation across substantive tests: **3.6–4.0 t/s**, above the project's 3.5 t/s target in these individual runs.
- Full Vulkan offload (`-ngl 100`) works.
- CPU and full Vulkan produced approximately the same generation range, so the model does not depend on GPU acceleration to reach its observed throughput.
- A 2048-token controlled context allowed normal short inference within the 4 GB system's memory envelope.

Important weaknesses:

- Chinese→English translation was only a partial pass because of tense and naturalness errors.
- Mixed English/Chinese conversational instruction following failed the tested requirement, including after clarification.
- The tested Chinese *Romance of the Three Kingdoms* summary produced major factual/chronological errors.
- The model's default context caused an attempted ~4.38 GiB allocation and failed on the 4 GB machine.
- A separate Vulkan driver segmentation fault was recorded during the broader testing session; the exact triggering invocation was not captured.
- Observed prompt processing of approximately 8.5–12.6 t/s is below the project's 30 t/s target, although prompt throughput has not yet been measured with a controlled fixed-length workload.
- Sustained 30-minute performance has not been tested.

## Qwen2.5-1.5B comparison position

Qwen2.5-1.5B-Instruct remains an important comparison candidate because it demonstrated stronger tested mixed-language recovery and substantial Vulkan acceleration.

Observed strengths:

- Coherent English and Simplified Chinese.
- Usable bidirectional translation.
- Mixed-language interaction recovered after clarification.
- Intel HD Graphics 500 Vulkan full offload worked.
- Observed Vulkan generation: **2.3–2.5 t/s**.

Important weaknesses:

- Below the project's later 3.5 t/s generation target.
- Larger 1.0 GiB model footprint than Qwen3-0.6B's 461.8 MiB.
- Mixed-language instruction following was initially unreliable.
- The tested Chinese historical-summary prompt contained major factual/chronological errors.

Qwen2.5-1.5B therefore remains a useful fallback/comparison model while Qwen3-0.6B is investigated further.

## Qwen2.5-0.5B baseline position

Qwen2.5-0.5B-Instruct remains useful as the low-resource reference point. It achieved approximately 2.1–2.6 t/s generation on CPU but showed weaker translation naturalness and repeatedly failed the tested mixed-language conversational task.

## Pending evaluation

The following candidates are deliberately not assigned numerical ranks until they have gone through the same Apollo Lake tests:

### Other lighter / ~1B candidates

- Hunyuan-0.5B-Instruct
- Chinese-Tiny-LLM (CT-LLM 0.9B)
- MiniCPM5-1B
- ZGCM-1 1.1B

### ~1.5–1.8B candidates

- Qwen2.5-Coder-1.5B
- DeepSeek-R1-Distill-Qwen-1.5B
- OpenCoder ~1.5B
- RWKV-6 ~1.6B
- InternLM2.5-1.8B-Chat

Additional 2B+ candidates may be added if the lighter candidates fail the linguistic/performance gates and a larger model has a strong technical justification.

## Ranking policy

The ranking will be updated as candidates complete comparable evaluation sequences. Performance numbers from different prompts or different configurations will not be treated as directly comparable unless the workload is controlled.

The project should avoid choosing a model solely because it is:

- smaller,
- larger,
- faster on a different machine,
- higher-ranked on a general benchmark, or
- more impressive on paper.

Apollo Lake's target hardware and actual bilingual assistant workload are the deciding test environment.

The current Qwen3-0.6B promotion is therefore a **provisional engineering position**, not a claim that it has already satisfied every project requirement.
