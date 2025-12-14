---
name: skill-reasoning-reconstruction-builder
description: Reconstructs plans and designs from validated truths using minimal viable structure, avoiding reintroduction of unvalidated assumptions. Combines first-principles reconstruction with simplicity-first design.
allowed-tools: Read
---

# Reconstruction Builder - Build From Validated Truths

## Overview

This skill builds solutions from fundamentals after deconstruction and assumption auditing. It emphasizes minimal viable structure: the simplest system that satisfies validated requirements and real constraints.

## Core Philosophy

- Start from truths, not patterns.
- Reintroduce assumptions only when explicitly labeled and scheduled for validation.
- Prefer layered, composable construction over monolithic designs.

## Core Capabilities

- Create a validated-truth inventory and minimal constraint set.
- Propose a minimal viable solution and staged enhancements.
- Identify new assumptions introduced by each layer.
- Define validation checkpoints and success metrics.

## When to Use

- After you’ve clarified the problem and mapped constraints.
- When you need a plan that is both rigorous and implementable.
- When you want innovation without chaos.

## Quick Start

**User**: “Design a new authorization system.”

**Response**:

- Truths: tenant model, resource types, performance needs, audit requirements.
- Minimal solution: centralized policy evaluation + explicit resource/tenant IDs.
- Layered add-ons: caching, policy authoring tools, analytics.
- Validate: unit tests + integration tests + audit log sampling.

## Output Format

- **Validated truths**: what’s known
- **Minimal solution**: the simplest design that satisfies truths
- **Layers**: optional enhancements with explicit assumptions
- **Validation plan**: tests/metrics/rollout steps

## Priority Hierarchy

1. **Correctness**: satisfy validated requirements and constraints
2. **Simplicity**: minimal parts, explicit boundaries, low coupling
3. **Validation**: assumptions tracked and tested
