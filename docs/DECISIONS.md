# Architectural Decisions

This document records important decisions made during Project Apollo Lake.

The purpose is to preserve the reasoning behind decisions so that they can be understood and revisited later.

---

## Decision 001 — GitHub as Project Source of Truth

**Date:** 2026-09-12

**Decision:**

The GitHub repository will act as the authoritative record of the project's code, architecture, experiments, decisions and learning.

**Reason:**

The project will involve multiple AI assistants and many development sessions. Relying on conversational memory alone could cause important information to be lost.

A persistent repository provides a common source of truth that can be understood by different AI systems and by humans.

---

## Decision 002 — Fully Offline Operation

**Date:** 2026-09-12

**Decision:**

The final assistant must operate without requiring an internet connection.

**Reason:**

Offline local operation is a fundamental project requirement and is central to the project's purpose.

---

## Decision 003 — Hardware Characterisation Before System Design

**Date:** 2026-09-12

**Decision:**

The Lenovo hardware will be characterised before making major operating-system, LLM and supporting AI component decisions.

**Reason:**

The hardware is extremely resource constrained. Software choices should therefore be based on measured hardware capabilities rather than assumptions.

---

## Decision 004 — AI-Assisted but Human-Understandable Development

**Date:** 2026-09-12

**Decision:**

AI assistants may provide substantial assistance, but important code, concepts and architectural decisions should be understood by the developer.

**Reason:**

The project is intended both as an engineering project and as a learning experience. The developer should be able to explain how the final system works.

---

## Decision 005 — Targeted Rather Than Exhaustive Benchmarking

**Date:** 2026-09-15

**Decision:**

Project Apollo Lake will use targeted benchmarking: perform only the measurements necessary to support hardware, operating-system, AI-runtime and architecture decisions.

**Reason:**

The project is intended to produce a working offline assistant on extremely constrained hardware. Excessive benchmarking would consume time without materially improving engineering decisions.

Benchmarking will therefore prioritise reproducibility, relevance and practical decision-making.

---

## Decision 006 — Integrated Endurance Requirement

**Date:** 2026-09-15

**Decision:**

The completed assistant should demonstrate at least 30 minutes of continuous local operation as an integrated system.

**Reason:**

Short component-level benchmarks cannot establish whether the complete microphone → ASR → LLM → TTS → memory pipeline can remain operational over sustained use.

Endurance testing will therefore be performed after the integrated system exists rather than during initial hardware characterisation.

---

## Decision 007 — LLM-First System Development

**Date:** 2026-09-17

**Decision:**

LLM selection will precede final supporting AI-stack selection. The project will find a satisfactory LLM before committing to the rest of the assistant architecture.

**Reason:**

The LLM is the central component of the assistant. The rest of the system should be designed around the actual resource requirements of the selected model rather than an assumed model or software environment.

---

## Decision 008 — Minimum Suitable LLM Rather Than Maximum Possible LLM

**Date:** 2026-09-17

**Decision:**

The target is the smallest practical LLM that provides acceptable conversational quality for Apollo Lake's requirements, with sufficient resources remaining for the rest of the offline assistant.

The project will not automatically select the largest model that can technically run on the Lenovo.

**Reason:**

A larger model is only useful if its additional capability justifies its additional resource requirements. Apollo Lake must also run speech recognition, text-to-speech and memory systems within the same 4 GB hardware constraint.

LLM testing will therefore proceed from very small models upward until a satisfactory model is found.

---

## Decision 009 — Alpine Linux as the Lightweight Reference Environment

**Date:** 2026-09-17

**Decision:**

Alpine Linux x86-64 will be used as the lightweight reference operating system for Phase 2 and subsequent Apollo Lake development.

The Lenovo will be reformatted and Alpine installed persistently to the internal eMMC. The Alpine environment will be kept deliberately minimal, with software added only when required by the project.

The operating-system question is temporarily considered settled. It will only be revisited if a concrete compatibility or engineering requirement demonstrates that Alpine is unsuitable.

**Reason:**

Alpine provides a very small base system while retaining the Linux software ecosystem required for Apollo Lake. The project has already successfully booted Alpine on the target Lenovo and experimentally verified key capabilities including low baseline RAM usage, SSH, CPU monitoring/frequency control, i915 graphics exposure and Mesa Vulkan support.

Continuing with Alpine avoids spending project time comparing many lightweight distributions while still providing a realistic environment in which to test the actual LLM and complete assistant workload.

The aim is therefore not to identify the theoretically smallest operating system, but to establish a practical lightweight environment and then focus engineering effort on the LLM and complete assistant.

---

## Decision 010 — Test iGPU Acceleration Rather Than Assume It

**Date:** 2026-09-17

**Decision:**

The Intel HD Graphics 500's available Vulkan capability will be tested as a possible LLM acceleration path, but GPU acceleration will not be assumed to improve performance.

Where practical, equivalent CPU-only and Vulkan-offloaded inference workloads will be compared using the same model, quantisation and relevant runtime settings.

**Reason:**

Phase 1 established that the N3350's Intel HD Graphics 500 is exposed through i915 and that Vulkan is available through Mesa. This establishes technical capability, not useful LLM performance.

Because the iGPU uses shared system memory and is itself highly constrained, actual measurements are required to determine whether offloading work to it is beneficial.
