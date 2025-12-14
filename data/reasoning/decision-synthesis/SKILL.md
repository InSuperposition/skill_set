---
name: skill-reasoning-decision-synthesis
description: Produces clear decisions with explicit reasoning chains, tradeoffs, and a simplicity-first recommendation grounded in validated truths. Integrates first-principles rigor with simplicity-based evaluation.
allowed-tools: Read
---

# Decision Synthesis - Make the Reasoning Explicit

## Overview

This skill synthesizes scattered inputs (requirements, constraints, options, risks) into a decision with a clear rationale. It is designed for decisions that need to be communicated, defended, and revisited.

It combines first principles (truth inventory, assumption control) with simplicity (choose low-coupling structures).

## Core Philosophy

- A decision without a rationale is a future argument.
- Prefer reversible, low-regret moves when uncertainty is high.
- Simplicity is a first-class criterion: reduce entanglement and future cost.

## Core Capabilities

- Summarize the problem, constraints, and success criteria.
- List options and the key tradeoffs.
- Identify assumptions and validation plans.
- Recommend a decision and articulate why it wins.
- Produce an “if wrong, then…” rollback/mitigation plan.

## When to Use

- Architecture/tech selection, migrations, platform choices.
- Product strategy and high-stakes tradeoffs.
- Any decision needing stakeholder alignment.

## Quick Start

**User**: “Should we build or buy?”

**Response**:

- Truths: requirements, timeline, budget, integration constraints.
- Options: build minimal core, buy with customization, hybrid.
- Evaluate: simplicity/coupling, operational burden, time-to-value, risk.
- Decision: pick the simplest path that meets constraints; add validation milestones.

## Output Format

- **Decision**: the chosen option (1 sentence)
- **Context**: problem + success criteria
- **Constraints**: real vs assumed
- **Options considered**: with pros/cons
- **Rationale**: key reasons grounded in validated truths
- **Assumptions + validation**: what must be tested
- **Rollback/mitigation**: how to recover if wrong

## Priority Hierarchy

1. **Clarity**: decision and rationale easy to restate
2. **Rigor**: grounded in truths; assumptions labeled
3. **Simplicity**: minimize coupling and complexity debt
