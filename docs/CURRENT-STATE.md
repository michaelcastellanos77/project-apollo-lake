# Current Project State

Last updated: 2026-09-12

## Current Phase

**Phase 0 — Project setup**

## Completed

- GitHub repository created
- Initial README created
- Shared AI context document created

## Hardware

Device:
Lenovo IdeaPad 120S-11IAP

RAM:
4 GB

Storage:
32 GB

CPU:
Not yet documented

Operating system:
Not yet selected

## AI System

LLM:
Not yet selected

Speech recognition:
Not yet selected

Text-to-speech:
Not yet selected

Memory system:
Not yet implemented

## Current Architecture

No implementation exists yet.

The intended high-level architecture is:

Microphone
→ Speech Recognition
→ Bilingual LLM
↕
Memory System
→ Text-to-Speech
→ Speaker

## Current Objective

Document the Lenovo hardware and establish a baseline before making architectural decisions.

## Next Action

Investigate and document the exact CPU, memory, storage and hardware capabilities of the Lenovo 120S-11IAP.

## Known Risks

- Extremely limited 4 GB RAM
- Very limited 32 GB storage
- Low-power Apollo Lake CPU
- Potentially slow CPU-only LLM inference
- Potentially difficult simultaneous ASR + LLM + TTS operation

## Open Questions

- Which exact CPU is installed?
- Which operating system is most appropriate?
- Which LLM can provide the best bilingual performance within the hardware limits?
- Which quantisation level provides the best quality/performance trade-off?
- Which ASR system is practical?
- Which TTS system is practical?
- What response latency is achievable?
