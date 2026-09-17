# Project Apollo Lake

A fully offline bilingual English/Mandarin voice assistant running locally on a Lenovo IdeaPad 120S-11IAP.

## Project Vision

The goal of Project Apollo Lake is to establish a fully independent local AI voice assistant on heavily constrained hardware. This project aims to teach me about how LLMs work and how LLM suitability and performance varies based on hardware used.

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
- Current test operating system: Ubuntu 26.04 LTS
- Final operating system: yet to be decided

## Project Philosophy

This project is being developed as a learning and engineering project rather than simply assembling a pre-existing AI application.

Important decisions, experiments, failures and lessons will be documented throughout development.

AI assistants may be used to help with research, programming, debugging and learning, but the project documentation will remain the authoritative record of the system.

The project prioritises finding a satisfactory practical LLM rather than maximising model size. Software choices will be made around the requirements of the complete assistant, not around the largest model the hardware can technically run.

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

Phase 1 is complete. Further benchmarking will be targeted only at measurements required for later engineering decisions.

The current test environment is Ubuntu 26.04 LTS. The final operating system has deliberately not yet been selected.

**Current focus: LLM research and selection.** Small bilingual LLMs are being tested from the bottom up to find the smallest model that provides satisfactory English, Simplified Chinese and mixed-language conversational capability.

If Ubuntu prevents a suitable model from running satisfactorily, a lighter operating system will be investigated and LLM testing will continue. Even if a suitable model is found on Ubuntu, a lighter operating system may later be evaluated to free resources for speech, memory and other components.

## Roadmap

- [x] Document Lenovo hardware
- [x] Establish targeted hardware performance baseline
- [ ] Research and test small bilingual LLMs
- [ ] Select initial LLM and model configuration
- [ ] Select/configure inference runtime
- [ ] Determine final operating system based on LLM and complete-system requirements
- [ ] Design memory architecture
- [ ] Implement short-term memory
- [ ] Implement persistent long-term memory
- [ ] Implement offline speech recognition
- [ ] Implement offline text-to-speech
- [ ] Integrate complete voice pipeline
- [ ] Optimise CPU, memory and thermals
- [ ] Establish final performance measurements
- [ ] Demonstrate at least 30 minutes of continuous local operation
- [ ] Document final architecture and evaluation

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

Further hardware benchmarking will only be performed where it supports a specific later engineering decision.

### Phase 2 — LLM Research and Selection

- [ ] Define practical LLM requirements
- [ ] Research extremely small bilingual models
- [ ] Verify English generation capability
- [ ] Verify Simplified Chinese generation capability
- [ ] Verify English ↔ Chinese interaction
- [ ] Verify mixed English/Chinese conversation
- [ ] Test candidate models on Ubuntu 26.04
- [ ] Measure practical RAM usage and generation performance
- [ ] Test relevant quantisation levels where useful
- [ ] Select the smallest practical LLM that meets project requirements

### Phase 3 — Operating System and Inference Runtime

- [ ] Determine operating-system requirements from the selected LLM and complete assistant architecture
- [ ] Continue with Ubuntu if it provides sufficient resources
- [ ] Investigate lighter operating systems if additional resources are required
- [ ] Select/configure the inference runtime
- [ ] Verify reliable fully offline LLM operation

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
