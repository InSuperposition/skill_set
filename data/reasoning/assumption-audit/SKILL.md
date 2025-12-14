---
name: skill-reasoning-assumption-audit
description: Surfaces and stress-tests hidden assumptions, labels uncertainty, and proposes the smallest validations. Uses first-principles rigor and simplicity to prevent assumption-stacking and complexity debt.
allowed-tools: Read
---

# Assumption Audit - Make the Implicit Explicit

## Overview

This skill identifies assumptions in plans, designs, arguments, and code paths, then helps validate or replace them. It blends:

- **First principles**: distinguish facts from assumptions and reduce reasoning to validated truths.
- **Simplicity**: remove assumption-driven features, abstractions, and constraints that add entanglement.

## Core Philosophy

- Most failures come from **unseen assumptions**, not bad intentions.
- Assumptions compound: stacked uncertainty produces fragile systems.
- Prefer a smaller, validated approach over a large, assumed one.

## Core Capabilities

- Extract assumptions from text, architecture, requirements, and “common sense”.
- Classify assumptions: user, technical, business, data, operational, legal.
- Mark confidence levels and what evidence would change your mind.
- Propose the smallest validation: measurement, prototype, log, test, spike, or interview.
- Replace assumptions with simpler constraints or designs when possible.

## When to Use

- “Everyone knows…” appears in the reasoning.
- Decisions rely on multiple “probably” statements.
- You’re about to commit to a large build or migration.
- Post-incident: you need to find which belief was wrong.

## Quick Start

**User**: “We should switch to X because it’ll be faster to develop.”

**Response**:

- Assumptions:
  - “Faster to develop” means fewer engineer-hours over 6 months.
  - Team has sufficient familiarity with X.
  - Migration risk is acceptable.
- Validation:
  - 2-day spike implementing one representative feature.
  - Compare LOC, complexity, and integration friction.
- Simplicity check:
  - Does X reduce entanglement, or just feel easier due to familiarity?

## Output Format

- **Assumptions list** (bulleted), each with:
  - **Type** (user/technical/business/etc.)
  - **Confidence** (low/medium/high)
  - **Evidence** (current)
  - **Validation** (smallest test)
- **Top 1–3 risky assumptions**: highest impact × lowest confidence
- **Recommended next step**: smallest action to validate or simplify

## Priority Hierarchy

1. **Rigor**: separate what’s known from what’s believed
2. **Simplicity**: remove assumptions by reducing scope and coupling
3. **Speed**: validate with the smallest credible experiment
