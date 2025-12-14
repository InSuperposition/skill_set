---
name: skill-reasoning-complexity-budget
description: Establishes a complexity budget and allocates it intentionally across requirements, constraints, and implementation. Uses first-principles decomposition and simplicity to minimize accidental complexity.
allowed-tools: Read
---

# Complexity Budget - Spend Complexity Intentionally

## Overview

This skill treats complexity as a scarce resource. It helps you decide where complexity is essential and where it is accidental, then keep the system within a deliberate budget.

It combines first principles (identify essentials) with simplicity (remove accidental complexity and entanglement).

## Core Philosophy

- Complexity is not free; it accrues interest in maintenance, bugs, and onboarding.
- Spend complexity only to satisfy validated requirements and real constraints.
- Prefer simple structure over cleverness.

## Core Capabilities

- Separate essential vs accidental complexity.
- Identify sources: requirements, integration boundaries, state, concurrency, configuration, abstractions.
- Define a budget: max concepts, max dependencies, max variants, max operational burden.
- Propose reductions: remove features, narrow scope, standardize, delete layers.

## When to Use

- Early design and architecture phases.
- Large refactors, platform work, or multi-team systems.
- Product plans that add many variants or edge cases.

## Quick Start

**User**: “We need this system to handle many use cases.”

**Response**:

- Which use cases are essential (validated demand) vs speculative?
- Budget: 1 primary workflow + 1 optional extension point.
- Reduce variants: standardize inputs, remove rarely used options.

## Output Format

- **Essential complexity**: what cannot be removed
- **Accidental complexity**: what can be redesigned away
- **Budget**: explicit caps (concepts, deps, variants, ops burden)
- **Cuts**: ranked simplifications to meet budget

## Priority Hierarchy

1. **Simplicity**: eliminate accidental complexity
2. **Rigor**: budget aligns with real constraints and requirements
3. **Maintainability**: optimize for future comprehension and change
