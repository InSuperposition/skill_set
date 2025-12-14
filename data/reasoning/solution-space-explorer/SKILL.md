---
name: skill-reasoning-solution-space-explorer
description: Expands and explores the solution space from validated fundamentals, avoiding default patterns and premature convergence. Combines first-principles reconstruction with simplicity-first evaluation.
allowed-tools: Read
---

# Solution Space Explorer - Don’t Default Too Early

## Overview

This skill helps you generate and evaluate multiple solution options built from validated truths (not from precedent). It prevents premature convergence on the first “reasonable” approach and uses simplicity as a selection criterion.

## Core Philosophy

- Defaults are often inherited assumptions in disguise.
- Generate options first; evaluate second.
- Prefer options that satisfy fundamentals with fewer parts and less coupling.

## Core Capabilities

- Start from a validated-truth inventory and real constraints.
- Generate diverse options (including “do nothing” and “reduce scope”).
- Compare options using: simplicity, risk, reversibility, cost, time-to-validate.
- Identify the smallest experiment to select between top options.

## When to Use

- Architecture and technology decisions.
- Product design with multiple plausible approaches.
- Refactors where the “standard” solution feels heavy.

## Quick Start

**User**: “We need to improve search.”

**Response**:

- Truths: query patterns, latency goals, dataset size, update frequency.
- Options: better indexing, caching, incremental improvements, change UX, add filters, specialized engine.
- Choose: simplest option that meets goals; test with a prototype.

## Output Format

- **Fundamentals**: validated truths + real constraints
- **Options**: 5–10, including “simplify requirements”
- **Evaluation matrix**: simplicity/coupling, risk, reversibility, cost
- **Recommendation**: top option + smallest validation step

## Priority Hierarchy

1. **Rigor**: options grounded in validated truths
2. **Simplicity**: prefer low-coupling, composable solutions
3. **Practicality**: choose options you can validate quickly
