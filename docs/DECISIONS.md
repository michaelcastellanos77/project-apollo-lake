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

LLM selection will precede final operating-system and supporting AI-stack selection.

The project will initially test small bilingual LLMs on the existing Ubuntu 26.04 environment. The goal is to find an LLM that provides satisfactory English and Simplified Chinese conversational capability, rather than to maximise model size.

If no suitable LLM can provide satisfactory results on Ubuntu 26.04, the project will investigate a lighter operating system and continue testing progressively larger models.

If a suitable LLM is found on Ubuntu 26.04, a lighter operating system may still be evaluated later if the additional resources would materially improve the speech, memory or other supporting components.

**Reason:**

The LLM is the central component of the assistant. Building the rest of the system around an assumed operating-system environment before establishing that a suitable LLM can run would risk optimising the wrong constraint.

The project therefore prioritises finding a satisfactory LLM first, while retaining operating-system optimisation as a later option.

---

## Decision 008 — Minimum Suitable LLM Rather Than Maximum Possible LLM

**Date:** 2026-09-17

**Decision:**

The target is the smallest practical LLM that provides acceptable conversational quality for Apollo Lake's requirements, with sufficient resources remaining for the rest of the offline assistant.

The project will not automatically select the largest model that can technically run on the Lenovo.

**Reason:**

A larger model is only useful if its additional capability justifies its additional resource requirements. Apollo Lake must also run speech recognition, text-to-speech and memory systems within the same 4 GB hardware constraint.

LLM testing will therefore proceed from very small models upward until a satisfactory model is found.
