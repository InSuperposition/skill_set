---
name: skill-reasoning-framework-feynman-technique
description: Uses the Feynman technique to explain ideas plainly, expose knowledge gaps, and simplify designs by reducing concepts and hidden dependencies.
allowed-tools: Read
---

# Feynman Technique - Test Understanding by Teaching Simply

## Overview

The Feynman technique verifies understanding by forcing a clear explanation in simple terms. It helps reveal gaps, circular definitions, and unnecessary complexity.

Source framework: `data/first-principles/frameworks/feynman-technique.md`.

## Core Principles

- If you can’t explain it simply, you don’t understand it yet.
- Replace jargon with definitions; reintroduce jargon only if it compresses well.
- Identify the minimal concept set needed to reason correctly.
- Use the explanation to simplify the underlying structure (not just the words).

## When to Use

- Writing design docs and RFCs
- Onboarding, teaching, knowledge transfer
- Debugging “magic” systems that few people can explain

## Operating Procedure

1. Explain the concept to a smart beginner (plain language).
2. Note where you get stuck or hand-wave.
3. Fill gaps with fundamentals, definitions, or examples.
4. Re-explain with fewer concepts and clearer boundaries.

## Output Format

- **Plain explanation**
- **Definitions** (minimal)
- **Concept inventory**
- **Gaps found** (and what would resolve them)
- **Simplification opportunities**

## Priority Hierarchy

1. **Clarity**: plain words and explicit definitions
2. **Correctness**: no misleading simplifications
3. **Simplicity**: fewer concepts, less hidden coupling
