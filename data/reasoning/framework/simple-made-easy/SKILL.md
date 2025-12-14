---
name: skill-reasoning-framework-simple-made-easy
description: Distinguishes objective simplicity from subjective ease, then guides decisions toward low-coupling, orthogonal designs with explicit tradeoffs.
allowed-tools: Read
---

# Simple Made Easy - Simple vs Easy as a Structural Check

## Overview

Rich Hickey’s “Simple Made Easy” separates objective simplicity (unentangled, orthogonal) from subjective ease (familiar, convenient). Use it to avoid “easy now, complex forever” decisions.

Source frameworks:

- `data/simplicity/frameworks/simple-made-easy.md`
- `data/first-principles/frameworks/simple-made-easy.md`

## Core Principles

- **Simple** is structural: fewer responsibilities per artifact, fewer couplings.
- **Easy** is contextual: familiarity, tooling, short-term effort.
- Choose simplicity first; add ease through tooling and ergonomics later.

## Simplicity Questions

- Is it singular (one conceptual job)?
- Is it orthogonal (change one part without touching others)?
- Is it composable (pieces connect without bespoke glue)?
- Is data inspectable and explicit?
- Is time/order/state explicit rather than implicit?

## When to Use

- Architecture comparisons and tech selection
- Refactors and abstraction proposals
- Decisions justified primarily by convenience

## Output Format

- **Options**
- **Simplicity vs ease** assessment per option
- **Coupling/entanglement hotspots**
- **Recommendation** (simplest viable option)
- **Make-it-easy plan** (tooling/ergonomics after)

## Priority Hierarchy

1. **Simplicity**: reduce entanglement and hidden coupling
2. **Clarity**: explicit data, dependencies, and time
3. **Practicality**: incremental steps to realize simplicity
