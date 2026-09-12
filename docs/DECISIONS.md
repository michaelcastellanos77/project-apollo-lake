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

## Decision 003 — Hardware-First Development

**Date:** 2026-09-12

**Decision:**

The Lenovo hardware will be investigated and benchmarked before selecting the final operating system, LLM and supporting AI components.

**Reason:**

The hardware is extremely resource constrained. Model and software choices should therefore be based on measured hardware capabilities rather than assumptions.

---

## Decision 004 — AI-Assisted but Human-Understandable Development

**Date:** 2026-09-12

**Decision:**

AI assistants may provide substantial assistance, but important code, concepts and architectural decisions should be understood by the developer.

**Reason:**

The project is intended both as an engineering project and as a learning experience. The developer should be able to explain how the final system works.
