# AI Context

This document is the shared context for AI assistants helping with Project Apollo Lake.

It is intended to allow different AI systems to understand the current state of the project without relying on previous conversations.

## Project Identity

Project name: Project Apollo Lake

Repository:
https://github.com/michaelcastellanos77/project-apollo-lake

## Project Purpose

Build a fully offline bilingual English/Mandarin voice assistant running entirely on a Lenovo IdeaPad 120S-11IAP.

This project will teach me more about how llms work and different models perform on the same hardware.

## Developer

Michael

University:
University College London (UCL)

Degree:
BSc Mathematics

Current year:
Year 2

Mandarin:
above HSK 5

## Device Hardware

Device:
Lenovo IdeaPad 120S-11IAP

- 2-core / 2-thread Intel Celeron N3350
- 4 GB physical RAM
- 3.66 GiB RAM visible to Linux
- Approximately 3.00 GiB available in the minimal Alpine environment during the RAM test
- No active swap in the Alpine benchmark environment
- AVX/AVX2 not reported
- Intel HD Graphics 500 exposed through i915
- Vulkan successfully enumerates the HD Graphics 500 through Mesa
- Internal SanDisk iNAND eMMC visible as approximately 29.1 GiB
- Raw sequential eMMC read: 161.2 MB/s
- Sustained 2-core CPU test: approximately 99.9% utilisation per core
- Maximum observed TCPU temperature during Experiment 003: 54°C
- No obvious thermal-frequency collapse was observed during the short controlled test


  
## Core Requirements

1. The system must operate fully offline.
2. The system must run exclusively on the Lenovo hardware.
3. The assistant must communicate naturally in English and Mandarin Chinese.
4. The assistant must support voice input and output.
5. The assistant must be able to support a minimum of 30 minutes of working conversation
6. The assistant must have persistent long-term memory.
7. The assistant must have rolling short-term conversational memory.
8. The system should achieve a reasonable conversational response time.
9. Available hardware resources should be utilised as effectively as possible.
10. The system should be benchmarked sufficiently to support hardware, software and architectural decisions, with important results documented and reproducible.

## Development Philosophy

This is a learning project.

AI assistants may help with:

- Research
- Programming
- Debugging
- Architecture
- Documentation
- Learning concepts
- Experiment design

However, the developer should understand the important concepts and decisions rather than blindly copying generated code.

The GitHub repository is the authoritative source of project state.

## Current Phase

Phase 2 — Choosing a suitable operating system.

## Current Objective

Establish a highly suitable operating system that balances functionality with low usage of system resouces.

## Important Historical Context

A previous attempt used TinyLlama on this laptop.

The result was unsatisfactory:

- Extremely slow response times
- Poor coherence
- Nonsensical responses

The new project should therefore prioritise benchmarking and hardware-aware model selection before building the complete assistant.

## Instructions for AI Assistants

When helping with this project:

- Read the current project documentation before making assumptions.
- Explain important concepts rather than only providing commands or code.
- Prefer simple, understandable solutions.
- Explain why a particular technology or architecture is being selected.
- Record important decisions and experiments in the repository.
- Do not assume that previous conversations are available.
- Treat the GitHub repository as the project's source of truth.
- Clearly distinguish established facts, measurements, assumptions and recommendations.


Project Apollo Lake's GitHub repository is the authoritative source of truth. When repository information is required, do not rely solely on the repository root page or search-result snippets. Navigate directly into the repository's files and folders, particularly docs/, and read the relevant files. If a file appears to be missing from the repository root, verify by directly opening the expected file path before concluding that it does not exist.

Repository: https://github.com/michaelcastellanos77/project-apollo-lake

Important current files include:

README.md
docs/AI-CONTEXT.md
docs/CURRENT-STATE.md
docs/DECISIONS.md
docs/LEARNING-NOTES.md
docs/hardware.md
experiments/001-controlled-baseline-initial-state.md
experiments/002-alpine-controlled-benchmark-environment.md
experiments/003-cpu-sustained-load-thermal.md


Never claim that a file is absent merely because it is not displayed on the repository root page. If the repository root appears inconsistent with the expected file structure, attempt to access the specific file directly. If direct access also fails, say that access failed rather than assuming the file does not exist.
