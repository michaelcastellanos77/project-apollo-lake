# Project Apollo Lake

A fully offline bilingual English/Mandarin voice assistant running locally on a Lenovo IdeaPad 120S-11IAP.

## Project Vision

The goal of Project Apollo Lake is to establish a fully independent local ai voice assistant on heavily constrained hardware. This project aims to teach me about how llms work and how llm suitability and performance varies based on hardware used.

The initial system will run entirely offline on a Lenovo IdeaPad 120S-11IAP and will not depend on cloud AI services during operation.

The project will investigate how capable a useful bilingual conversational AI system can become when running on very limited hardware.

## Core Goals

- Fully offline operation
- Natural English and Mandarin Chinese conversation
- Seamless bilingual communication
- Offline speech recognition
- Offline text-to-speech
- Persistent long-term user memory
- Rolling short-term conversational memory
- Reasonable conversational response time
- Maximum practical utilisation of the available hardware
- Targeted benchmarking and optimisation based on project requirements
- Minimum of 30 minutes continuous conversation

## Target Hardware

**Lenovo IdeaPad 120S-11IAP**

- CPU: Intel Celeron N3350
- RAM: 4 GB
- Storage: 32 GB
- Operating system: yet to be decided

## Project Philosophy

This project is being developed as a learning and engineering project rather than simply assembling a pre-existing AI application.

Important decisions, experiments, failures and lessons will be documented throughout development.

AI assistants may be used to help with research, programming, debugging and learning, but the project documentation will remain the authoritative record of the system.

## Current Status

**Phase 1 — Hardware characterisation complete**

The Lenovo IdeaPad 120S-11IAP has been characterised using a controlled minimal Linux environment.

Key findings include:
- 4 GB installed RAM, with approximately 3.66 GiB visible to Linux
- Intel Celeron N3350, 2 cores / 2 threads
- No AVX/AVX2 reported
- Intel HD Graphics 500 exposed through `i915`
- Vulkan successfully exposed through Mesa
- Internal 32 GB SanDisk iNAND eMMC
- Raw sequential eMMC read measured at 161.2 MB/s
- Short sustained CPU testing reached approximately 54°C maximum CPU temperature without observed thermal collapse

The final operating system and AI software stack have not yet been selected.

## Roadmap

- [V] Document Lenovo hardware
- [V] Establish performance baseline
- [ ] Investigate operating systems
- [ ] Investigate local LLMs
- [ ] Select initial bilingual LLM
- [ ] Design memory architecture
- [ ] Implement short-term memory
- [ ] Implement persistent long-term memory
- [ ] Implement offline speech recognition
- [ ] Implement offline text-to-speech
- [ ] Integrate complete voice pipeline
- [ ] Optimise CPU, memory and thermals
- [ ] Establish final performance benchmarks
- [ ] Document final architecture
