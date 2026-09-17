# Apollo Lake LLM Ranking

**Last updated:** 2026-09-17  
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
| **1** | **Qwen2.5-1.5B-Instruct Q4_K_M** | 1.5B | **Current leader** | **2.3–2.5 t/s Vulkan** | Strong English/Chinese; usable translation; mixed-language recovery after clarification; Vulkan full offload works |
| **2** | **Qwen2.5-0.5B-Instruct Q4_K_M** | 0.5B | Evaluated baseline | **2.1–2.6 t/s CPU** | Runs reliably; weaker translation naturalness and failed tested mixed-language interaction |

**Important:** the ranking is provisional. Qwen2.5-1.5B is the current leader because it is the strongest candidate demonstrated so far, but it has **not** met the later performance gate of 3.5 t/s generation or the 30 t/s prompt-processing target.

## Performance gate

Once a model passes the five linguistic/functional requirements, Apollo Lake applies the following performance target:

- **Generation:** ≥ 3.5 t/s
- **Prompt processing:** ≥ 30 t/s

A candidate should not be considered to meet these targets from a single unusually fast prompt. Prompt throughput must be measured using controlled prompts of known length/content, and generation should be confirmed across representative bilingual/conversational prompts.

The current leader therefore remains a **provisional leader**, not a final selection.

## Pending evaluation

The following candidates are deliberately not assigned numerical ranks until they have gone through the same Apollo Lake tests:

### Next priority

- **Qwen3-0.6B**

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

## Qwen2.5-1.5B current position

Qwen2.5-1.5B-Instruct is currently first because it is the strongest **empirically demonstrated** candidate so far, not because parameter count alone determines quality.

Observed strengths:

- Fits with swap disabled in the ~3.7 GiB physical-RAM environment.
- Coherent English generation.
- Coherent Simplified Chinese generation.
- Usable Chinese↔English translation, with some omissions/naturalness issues.
- Mixed-language interaction recovered successfully after clarification.
- Intel HD Graphics 500 Vulkan full offload (`-ngl 99`) works for this model in the persistent environment.
- Observed Vulkan generation: **2.3–2.5 t/s**.

Important weaknesses:

- Current observed generation is below the later **3.5 t/s minimum** target.
- Prompt throughput has not yet been established as a controlled model-level figure; recorded values depend strongly on prompt length/content.
- Initial mixed-language instruction following was unreliable.
- A tested Chinese *Romance of the Three Kingdoms* summary contained major factual/chronological errors.

Therefore Qwen2.5-1.5B is the **current leader, not yet the final model**.

## Qwen2.5-0.5B baseline position

Qwen2.5-0.5B-Instruct remains useful as the low-resource reference point. It achieved approximately 2.1–2.6 t/s generation on CPU but showed weaker translation naturalness and repeatedly failed the tested mixed-language conversational task.

## Ranking policy

The ranking will be updated only after a candidate has completed the comparable evaluation sequence. Performance numbers from different prompts or different configurations will not be treated as directly comparable unless the workload is controlled.

The project should avoid choosing a model solely because it is:

- smaller,
- larger,
- faster on a different machine,
- higher-ranked on a general benchmark, or
- more impressive on paper.

Apollo Lake's target hardware and actual bilingual assistant workload are the deciding test environment.
