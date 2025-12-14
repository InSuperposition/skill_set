---
name: skill-reasoning-framework-unix-philosophy
description: Applies the Unix philosophy (“do one thing well”) to decompose systems into composable components with explicit interfaces and minimal coupling.
allowed-tools: Read
---

# Unix Philosophy - Compose Small, Clear Parts

## Overview

The Unix philosophy is a decomposition and interface discipline: build small components with clear responsibilities and make them compose through explicit interfaces.

Source framework: `data/simplicity/frameworks/unix-philosophy.md`.

## Core Principles

- Do one thing well; avoid responsibility mixing.
- Prefer composition and pipes/contracted interfaces over tight integration.
- Make data and behavior easy to inspect.
- Text/structured data as an interchange format when appropriate.

## When to Use

- Designing modules, services, and internal tooling
- Refactoring monoliths and large subsystems
- Workflow and pipeline design

## Output Format

- **Responsibilities** (current)
- **Proposed components** (singular jobs)
- **Interfaces/contracts** (inputs/outputs)
- **Composition plan** (how pieces connect)
- **Coupling reductions** (edges removed)

## Priority Hierarchy

1. **Simplicity**: singular responsibilities and low coupling
2. **Composability**: explicit contracts
3. **Operability**: easy inspection and debugging
