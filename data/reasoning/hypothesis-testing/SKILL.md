---
name: skill-reasoning-hypothesis-testing
description: Converts beliefs into testable hypotheses and designs the smallest credible experiments. Uses first-principles validation and simplicity to avoid overbuilding before learning.
allowed-tools: Read
---

# Hypothesis Testing - Learn Before You Build

## Overview

This skill transforms “I think” into “We can test” by creating clear hypotheses, predictions, and minimal experiments. It draws from first principles (validate assumptions) and simplicity (do the smallest thing that teaches you something).

## Core Philosophy

- A hypothesis must be falsifiable: it should be possible to be wrong.
- Prefer experiments that reduce uncertainty with minimal coupling and cost.
- Learning is an output; features are a means.

## Core Capabilities

- Extract hypotheses from proposals, roadmaps, and arguments.
- Define measurable predictions and success thresholds.
- Design minimal experiments: prototype, A/B, spike, log, survey, shadow traffic.
- Identify confounders and how to control them.
- Decide: proceed, pivot, or stop based on results.

## When to Use

- Product bets and roadmap decisions.
- Performance or reliability “hunches”.
- Architecture migrations justified by predicted outcomes.

## Quick Start

**User**: “Adding caching will fix our latency.”

**Response**:

- Hypothesis: “Caching endpoint X reduces p95 from 1200ms to 400ms.”
- Prediction: cache hit rate > 80% on key set.
- Experiment: add metrics first; enable cache for 10% traffic; compare.
- Simplicity check: ensure caching doesn’t add invalidation complexity beyond benefit.

## Output Format

- **Hypothesis**: explicit statement
- **Prediction**: metric + threshold + timeframe
- **Experiment**: smallest credible test
- **Risks/Confounders**: what could mislead results
- **Decision rule**: what you’ll do for pass/fail/inconclusive

## Priority Hierarchy

1. **Testability**: falsifiable and measurable
2. **Rigor**: control for obvious confounders
3. **Simplicity**: minimal experiment and minimal new coupling
