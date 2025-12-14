---
name: skill-reasoning-constraint-mapping
description: Identifies and classifies constraints (real vs assumed), then redesigns within the smallest real constraint set. Uses first-principles constraint analysis and simplicity to avoid self-imposed complexity.
allowed-tools: Read
---

# Constraint Mapping - Real Limits vs Assumed Limits

## Overview

This skill maps constraints that shape a solution, explicitly distinguishing:

- **Real constraints**: physics, laws, hard requirements, proven resource limits
- **Assumed constraints**: conventions, preferences, legacy inertia, “we can’t”

It uses first principles to validate constraints and simplicity to minimize constraint-driven complexity.

## Core Philosophy

- Many “constraints” are merely unchallenged defaults.
- Fewer constraints means a larger solution space.
- Prefer constraints that preserve simplicity and composability.

## Core Capabilities

- Elicit constraints from stakeholders and documents.
- Classify constraints: hard, soft, reversible, irreversible.
- Challenge assumed constraints with targeted questions.
- Suggest constraint-relaxation experiments (prototypes, policy checks, spikes).
- Produce a “minimal constraint set” for solution exploration.

## When to Use

- You’re stuck: “no solution seems possible”.
- Design reviews are dominated by “we must…”.
- You’re planning a migration or architecture change.
- You want to explore novel options without breaking reality.

## Quick Start

**User**: “We can’t improve latency because the database is slow.”

**Response**:

- Real constraints: network latency exists; current DB p95 query time is X (if measured).
- Assumed constraints: “must fetch all fields synchronously”; “must use 12 queries”.
- Minimal constraint set: auth-first + 3 critical fields under 500ms perceived.
- Options: progressive loading, caching, denormalization, query reduction.

## Output Format

- **Constraints**:
  - **Hard** (must hold) vs **soft** (preference)
  - **Real** (validated) vs **assumed** (unvalidated)
- **Minimal real constraints**: the set you will not violate
- **Assumption tests**: smallest checks to confirm/deny assumed constraints
- **Implications**: what each constraint forces or forbids

## Priority Hierarchy

1. **Reality**: do not violate hard real constraints
2. **Rigor**: challenge assumed constraints explicitly
3. **Simplicity**: prefer solutions that reduce entanglement and moving parts
