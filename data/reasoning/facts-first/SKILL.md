---
name: skill-reasoning-facts-first
description: Rewrites messy situations into a facts-first brief: measured observations, definitions, and claims with evidence. Uses first-principles decomposition and simplicity to reduce confusion and improve decision quality.
allowed-tools: Read
---

# Facts First - Separate Observations From Interpretations

## Overview

This skill produces a clean “what do we actually know?” view of a problem by separating:

- **Observations** (measured, logged, witnessed)
- **Interpretations** (inferences, explanations)
- **Preferences** (values, tradeoffs)
- **Unknowns** (missing data)

It uses first principles to ground reasoning and simplicity to reduce narrative clutter.

## Core Philosophy

- If you can’t point to evidence, label it as an assumption.
- Define terms precisely before debating conclusions.
- Reduce the story to the smallest set of relevant facts.

## Core Capabilities

- Convert a narrative description into a structured facts-first brief.
- Flag undefined terms and replace with explicit definitions.
- Identify measurement gaps and propose low-effort instrumentation.
- Clarify the claim being made and what would falsify it.

## When to Use

- Meetings are full of opinions but light on data.
- Bugs or incidents have competing explanations.
- Strategy debates are stuck in “belief vs belief”.
- You need to write a decision memo or incident summary.

## Quick Start

**User**: “The new checkout is worse. Customers hate it.”

**Response**:

- Observations: conversion down 1.8% week-over-week; support tickets up 12%.
- Interpretations: “customers hate it” (unproven); could be slower load time.
- Unknowns: cohort breakdown; device split; error rate; step-level drop-offs.
- Next measurement: instrument step completion + client-side perf.

## Output Format

- **Definitions**: key terms (“worse”, “hate”, “slow”) made precise
- **Observations**: measured facts (with source if provided)
- **Claims**: what is being asserted
- **Evidence**: what supports the claim (or “none”)
- **Alternative explanations**: 2–3 plausible causes
- **Unknowns + next measurements**: smallest data to collect

## Priority Hierarchy

1. **Truthfulness**: do not upgrade guesses to facts
2. **Clarity**: explicit definitions and claims
3. **Simplicity**: minimal set of facts that matter
