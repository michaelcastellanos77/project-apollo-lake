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



## Project Phases

The project is divided into distinct phases so that major engineering decisions are made sequentially and documented before progressing to the next stage.

### Phase 0 — Project Start and Hardware Identification

- [x] GitHub repository created
- [x] Initial README created
- [x] Shared AI context document created
- [x] Gathered information on the specific hardware of the Lenovo IdeaPad 120S-11IAP
- [x] Established the project documentation and experiment structure

### Phase 1 — Hardware Performance Characterisation

- [x] Establish a controlled benchmarking environment
- [x] Characterise CPU capabilities
- [x] Measure sustained CPU performance
- [x] Characterise CPU thermal behaviour
- [x] Characterise available RAM
- [x] Characterise internal eMMC storage
- [x] Characterise integrated GPU and available acceleration APIs
- [x] Document results and limitations
- [x] Establish a practical hardware performance baseline

### Phase 2 — Operating System Selection

- [ ] Define operating-system requirements
- [ ] Compare suitable operating-system candidates
- [ ] Evaluate compatibility with the measured hardware
- [ ] Select the final operating system
- [ ] Install and configure the selected operating system

### Phase 3 — LLM Selection and Inference Runtime

- [ ] Define requirements for the local language model
- [ ] Identify suitable candidate models
- [ ] Select an appropriate inference runtime
- [ ] Benchmark candidate LLMs on the Lenovo
- [ ] Evaluate bilingual English/Mandarin capability
- [ ] Select the initial LLM and model configuration

### Phase 4 — Speech, Memory and Assistant Pipeline

- [ ] Select offline speech-recognition system
- [ ] Select offline text-to-speech system
- [ ] Design short-term conversational memory
- [ ] Design persistent long-term memory
- [ ] Implement memory system
- [ ] Integrate speech recognition, LLM inference, memory and text-to-speech
- [ ] Establish a functional bilingual voice-assistant pipeline

### Phase 5 — Final Optimisation

- [ ] Identify system bottlenecks
- [ ] Optimise CPU and memory usage
- [ ] Optimise model/runtime configuration
- [ ] Optimise storage usage and access where practical
- [ ] Optimise thermal behaviour where practical
- [ ] Establish final system performance measurements
- [ ] Demonstrate at least 30 minutes of continuous local operation

### Phase 6 — Project Completion and Evaluation

- [ ] Finalise the system architecture
- [ ] Document the final implementation
- [ ] Consolidate experiments and benchmark results
- [ ] Evaluate the system against the original project requirements
- [ ] Document limitations and lessons learned
- [ ] Document potential future improvements
- [ ] Complete final project review

