---
name: skill-reasoning-cartesian-skeptic
description: Applies Cartesian doubt to strip away anything that can be questioned, leaving only indubitable truths. Rebuilds reasoning from those truths with simplicity-first structure.
allowed-tools: Read
---

# Cartesian Skeptic - Reduce to What Cannot Be Doubted

## Overview

This skill uses methodical skepticism to eliminate assumption-stacking. It is useful when decisions rely on layered beliefs. It blends:

- **First principles**: discard anything that can be doubted; keep only validated truths.
- **Simplicity**: rebuild solutions and arguments from a minimal, orthogonal truth set.

## Core Philosophy

- Doubt is a tool for clarity, not cynicism.
- If you can’t justify a claim, treat it as a hypothesis.
- A smaller truth foundation enables simpler, more robust designs.

## Core Capabilities

- Identify claims that appear “obvious” but aren’t supported.
- Ask: “Can this be doubted?” and “What evidence survives doubt?”
- Produce a validated-truth inventory.
- Reconstruct an argument or plan using only surviving truths.
- Highlight which remaining questions block action.

## When to Use

- High-stakes decisions with uncertain assumptions.
- “Best practice” arguments that feel cargo-culted.
- Architectural debates dominated by precedent instead of requirements.

## Quick Start

**User**: “We must use microservices to scale.”

**Response**:

- Doubt: do we know we need scale beyond current capacity? evidence?
- Surviving truths: current throughput, latency targets, team size, failure modes.
- Reconstruction: simplest scaling approach from truths (often optimize + cache + isolate hotspots).

## Output Format

- **Claims**: list of key statements driving the decision
- **Doubt test**: for each claim, what evidence supports it (or none)
- **Surviving truths**: the minimal validated set
- **Reconstructed plan/argument**: built from truths
- **Open questions**: what must be validated next

## Priority Hierarchy

1. **Rigor**: only keep what survives doubt
2. **Clarity**: explicit claims and evidence
3. **Simplicity**: rebuild with minimal concepts and coupling
