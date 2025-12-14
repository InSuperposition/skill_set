---
name: skill-reasoning-dependency-graphing
description: Builds a dependency map (conceptual, process, or technical) to find critical paths, hidden coupling, and simplification opportunities. Integrates first-principles decomposition with simplicity-first decoupling.
allowed-tools: Read
---

# Dependency Graphing - See the Coupling

## Overview

This skill turns “it’s complicated” into an explicit dependency graph so you can:

- Identify the **critical path**
- Separate **required** dependencies from **incidental** ones
- Find seams to reduce entanglement and improve composability

It combines first-principles deconstruction (break down) with simplicity (reduce coupling).

## Core Philosophy

- If dependencies are implicit, complexity is unbounded.
- The graph is the system: optimize the structure, not the story.
- Prefer fewer edges (couplings) over fewer nodes (features).

## Core Capabilities

- Create dependency graphs for processes, systems, and arguments.
- Classify edges: requires, assumes, optional, temporal, data, authority.
- Identify cycles and high-degree nodes (complexity hotspots).
- Propose decoupling: queues, layering, explicit interfaces, “seams”.
- Recommend simplifications: remove nodes/edges that aren’t essential.

## When to Use

- “We can’t change X without breaking Y.”
- Systems are hard to test or reason about.
- Deployments have long chains of prerequisites.
- You need to plan refactoring or modularization.

## Quick Start

**User**: “Why is this feature so hard to ship?”

**Response**:

- Build a graph: feature → auth → identity provider → DB → legacy service → UI.
- Identify critical path + cycles.
- Remove accidental edges: make legacy call async; move enrichment behind cache.
- Outcome: smaller critical path, fewer coupled changes.

## Output Format

- **Nodes**: components/steps/concepts
- **Edges**: “A requires B because …”
- **Critical path**: the minimal sequence that must succeed
- **Coupling hotspots**: cycles, fan-in/fan-out, hidden dependencies
- **Simplification moves**: 3–5 concrete decoupling options

## Priority Hierarchy

1. **Explicitness**: make dependencies visible and precise
2. **Simplicity**: remove edges and cycles; create orthogonal components
3. **Practicality**: propose incremental decoupling steps
