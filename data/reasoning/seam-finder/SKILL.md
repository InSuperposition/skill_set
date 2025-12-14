---
name: skill-reasoning-seam-finder
description: Finds seams to split responsibilities, reduce coupling, and enable incremental change. Uses first-principles decomposition and simplicity-first design to create orthogonal components.
allowed-tools: Read
---

# Seam Finder - Create Separation Where It Matters

## Overview

This skill identifies “seams” in systems, processes, and designs: places where you can split responsibilities to reduce entanglement. Seams make refactors safer, testing easier, and reasoning clearer.

It combines first-principles decomposition (break into parts) with simplicity (orthogonality and composability).

## Core Philosophy

- Entanglement is the enemy of change.
- Seams should follow real boundaries: data ownership, timing, trust, failure domains.
- Prefer seams that reduce dependency edges and hidden state.

## Core Capabilities

- Identify responsibility mixtures (auth + business logic + IO + formatting).
- Propose seam types: interface boundaries, module boundaries, queues, adapters, feature flags.
- Suggest incremental extraction plans with safe checkpoints.
- Detect seam anti-patterns: “abstraction layers” that hide coupling without reducing it.

## When to Use

- Refactoring legacy systems.
- Modularizing a codebase or service boundary.
- Making systems more testable and maintainable.

## Quick Start

**User**: “This module is impossible to test.”

**Response**:

- Find entanglement: business logic depends on DB + network + time + global config.
- Seams: dependency injection for IO, pure function core, explicit config object.
- Plan: extract pure core first, then adapt IO around it.

## Output Format

- **Where it’s entangled**: concrete examples of mixed responsibilities
- **Candidate seams**: 3–7, with why each helps
- **Incremental plan**: smallest safe steps to introduce seams
- **Success signals**: what gets easier (tests, changes, deploys)

## Priority Hierarchy

1. **Simplicity**: reduce coupling and implicit dependencies
2. **Clarity**: explicit boundaries and responsibilities
3. **Incrementality**: safe steps that preserve behavior
