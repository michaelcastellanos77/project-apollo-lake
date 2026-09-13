# Learning Notes

This document records concepts learned during the development of Project Apollo Lake.

The purpose is to build a personal technical knowledge base rather than simply record the final implementation.

---
Key Vocabulary:


# Repository
A repository is the main project space containing files and their version history.

# Commit
A commit is a recorded checkpoint in the project's history.

# Parallelism
The ability for a hardware component to execute multiple instructions or tasks at the exact same time using separate sub components

# CPU Cache
Hardware designed to give the CPU access to frequently used data very fast - faster than RAM can

# ISA (Instruction Set Architecture)
a standardised technical language/specification that compatible CPUs agree to understand

# Source Code
Code written by a human in a particular coding language that can be understood by humans

# Compiler
A compiler is software that translates source code into an ISA for a hardware component (that can execute instructions) like a cpu, gpu or npu to understand

# Neural Network
A neural network is a mathematical function, usually composed of many layers of mathematical operations, whose behaviour is controlled by a large collection of numerical parameters

# LLM (Large Language Model)
An LLM is a neural network trained to model and generate language, operating internally on numerical representations of text

# Training
Training is the process of repeatedly giving a neural network examples from a dataset, measuring how wrong its output is, and adjusting its parameters so that its future outputs become more accurate/useful

# Dataset
A dataset is a structured collection of data used to teach, test, or evaluate an AI model

# Parameter
Parameters are the internal settingsof a neural network that control how it processes inputs, directly affecting how accurately it can return a useful output. (Think co-efficients for a maclaurin series)

# Weight
Weight is the specific parameter that multiplies the input data, determining how heavily that specific piece of information impacts the next output or next layer

# Inference
Inference is the process of using a trained neural network to produce an output from an input. In other words, executing the trained neural network on an input

# Inference Runtime/ Inference Engine
An inference runtime is software that loads a trained model and provides the machinery needed to execute its mathematical operations on available hardware

# Token
A token is the basic unit of text that an AI can read or write

# Tokeniser
A tokeniser is the software tool (the translator) that splits your raw string of text into these tokens, and then maps each token to a specific, unique number.
