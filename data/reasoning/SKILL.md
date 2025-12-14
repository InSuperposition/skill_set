---
name: skill-reasoning
description: Generalized reasoning skillset with 5 core operational modes plus framework modes. Indexes the reasoning subskills (including frameworks) in this repo and guides when to use each.
allowed-tools: Read, Write, Grep, Glob, Bash, TodoWrite
---

# Reasoning - Unified Thinking Skillset

## Overview

This skill is the generalized entry point for structured reasoning in this repo. It turns ambiguity into clarity, reduces assumption-stacking, expands the solution space from fundamentals, and produces decisions with explicit rationale.

It includes 5 core operational modes (derived from the reasoning subskills) and additional framework modes (derived from `data/reasoning/framework/`).

## Requirements

- No external dependencies required

## Operating Principles

- **Facts before stories**: label observations vs interpretations vs assumptions.
- **Make constraints explicit**: distinguish real constraints from assumed constraints.
- **Prefer falsifiable claims**: turn beliefs into tests and predictions.
- **Simplicity first**: reduce coupling and complexity debt; choose the smallest viable structure.
- **Document rationale**: decisions should be explainable and revisitable.

## Operational Modes (5)

### 1) Frame Mode

Purpose: define the problem precisely, establish success criteria, and separate facts from interpretation.

Use subskills: `skill-reasoning-problem-framing`, `skill-reasoning-facts-first`.

### 2) Challenge Mode

Purpose: surface assumptions, search for counterexamples, and reduce reasoning to what survives scrutiny.

Use subskills: `skill-reasoning-assumption-audit`, `skill-reasoning-counterexample-hunter`, `skill-reasoning-cartesian-skeptic`.

### 3) Map Mode

Purpose: make structure visible (constraints, dependencies, seams) to find leverage points and reduce entanglement.

Use subskills: `skill-reasoning-constraint-mapping`, `skill-reasoning-dependency-graphing`, `skill-reasoning-seam-finder`.

### 4) Build Mode

Purpose: generate options from fundamentals, design minimal experiments, and reconstruct solutions incrementally.

Use subskills: `skill-reasoning-solution-space-explorer`, `skill-reasoning-hypothesis-testing`, `skill-reasoning-reconstruction-builder`.

### 5) Decide Mode

Purpose: synthesize tradeoffs into a clear recommendation with explicit rationale and a validation/rollback plan.

Use subskills: `skill-reasoning-decision-synthesis`, `skill-reasoning-simplicity-sanity-check`, `skill-reasoning-complexity-budget`.

## Framework Modes

These are “drop-in” reasoning frameworks you can invoke as modes when you want a specific questioning or simplification structure:

- `skill-reasoning-framework-five-whys`
- `skill-reasoning-framework-socratic-method`
- `skill-reasoning-framework-cartesian-doubt`
- `skill-reasoning-framework-feynman-technique`
- `skill-reasoning-framework-simple-made-easy`
- `skill-reasoning-framework-dieter-rams`
- `skill-reasoning-framework-kiss-principle`
- `skill-reasoning-framework-laws-of-simplicity`
- `skill-reasoning-framework-unix-philosophy`
- `skill-reasoning-framework-galls-law`

## Standard Workflow

1. **Frame**: problem statement, success criteria, constraints, non-goals.
2. **Challenge**: assumptions and counterexamples; what must be validated.
3. **Map**: dependencies and seams; where coupling creates risk.
4. **Build**: generate options; pick experiments; reconstruct from truths.
5. **Decide**: recommendation, rationale, and a plan to validate/iterate.

## Output Format (Default)

- **Problem statement** + **success criteria**
- **Facts / assumptions / unknowns**
- **Constraints** (real vs assumed)
- **Options** (including “simplify requirements”)
- **Recommendation** + **rationale**
- **Validation plan** (tests/experiments/measurements)

## References

### Core Reasoning Subskills

- `skill-reasoning-problem-framing`: `data/reasoning/problem-framing/SKILL.md`
- `skill-reasoning-facts-first`: `data/reasoning/facts-first/SKILL.md`
- `skill-reasoning-assumption-audit`: `data/reasoning/assumption-audit/SKILL.md`
- `skill-reasoning-counterexample-hunter`: `data/reasoning/counterexample-hunter/SKILL.md`
- `skill-reasoning-cartesian-skeptic`: `data/reasoning/cartesian-skeptic/SKILL.md`
- `skill-reasoning-constraint-mapping`: `data/reasoning/constraint-mapping/SKILL.md`
- `skill-reasoning-dependency-graphing`: `data/reasoning/dependency-graphing/SKILL.md`
- `skill-reasoning-seam-finder`: `data/reasoning/seam-finder/SKILL.md`
- `skill-reasoning-solution-space-explorer`: `data/reasoning/solution-space-explorer/SKILL.md`
- `skill-reasoning-hypothesis-testing`: `data/reasoning/hypothesis-testing/SKILL.md`
- `skill-reasoning-reconstruction-builder`: `data/reasoning/reconstruction-builder/SKILL.md`
- `skill-reasoning-decision-synthesis`: `data/reasoning/decision-synthesis/SKILL.md`
- `skill-reasoning-simplicity-sanity-check`: `data/reasoning/simplicity-sanity-check/SKILL.md`
- `skill-reasoning-complexity-budget`: `data/reasoning/complexity-budget/SKILL.md`
- `skill-reasoning-root-cause-tracing`: `data/reasoning/root-cause-tracing/SKILL.md`
- `skill-reasoning-socratic-clarifier`: `data/reasoning/socratic-clarifier/SKILL.md`
- `skill-reasoning-feynman-explainer`: `data/reasoning/feynman-explainer/SKILL.md`

### Framework Subskills

- `skill-reasoning-framework-five-whys`: `data/reasoning/framework/five-whys/SKILL.md`
- `skill-reasoning-framework-socratic-method`: `data/reasoning/framework/socratic-method/SKILL.md`
- `skill-reasoning-framework-cartesian-doubt`: `data/reasoning/framework/cartesian-doubt/SKILL.md`
- `skill-reasoning-framework-feynman-technique`: `data/reasoning/framework/feynman-technique/SKILL.md`
- `skill-reasoning-framework-simple-made-easy`: `data/reasoning/framework/simple-made-easy/SKILL.md`
- `skill-reasoning-framework-dieter-rams`: `data/reasoning/framework/dieter-rams/SKILL.md`
- `skill-reasoning-framework-kiss-principle`: `data/reasoning/framework/kiss-principle/SKILL.md`
- `skill-reasoning-framework-laws-of-simplicity`: `data/reasoning/framework/laws-of-simplicity/SKILL.md`
- `skill-reasoning-framework-unix-philosophy`: `data/reasoning/framework/unix-philosophy/SKILL.md`
- `skill-reasoning-framework-galls-law`: `data/reasoning/framework/galls-law/SKILL.md`
