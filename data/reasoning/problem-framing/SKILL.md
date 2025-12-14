---
name: skill-reasoning-problem-framing
description: Turns vague prompts into crisp problem statements with measurable success criteria, boundaries, and minimal scope. Uses first-principles clarification and simplicity-first scoping to make problems solvable.
allowed-tools: Read
---

# Problem Framing - Define What We’re Solving

## Overview

This skill helps you turn ambiguity into a precise, solvable problem statement. It combines:

- **First principles**: clarify the goal, separate the problem from surrounding context, and define success before proposing solutions.
- **Simplicity**: aggressively minimize scope, concepts, and moving parts so the problem remains comprehensible and tractable.

## Core Philosophy

- If the problem isn’t stated clearly, the “solution” is guesswork.
- Prefer a **narrow, testable** definition over a broad, inspirational one.
- Define **non-goals** to prevent accidental complexity.

## Core Capabilities

- Convert vague goals into measurable outcomes.
- Identify stakeholders and the single “decision owner” when relevant.
- Define boundaries: what is in-scope, out-of-scope, and unknown.
- Distinguish real constraints from assumed constraints.
- Produce the minimal next questions needed to proceed.

## When to Use

- Requirements are fuzzy, conflicting, or “hand-wavy”.
- You keep debating solutions without agreement on the problem.
- The project feels big and chaotic; you need a smaller target.
- You need a crisp statement for planning, alignment, or documentation.

## Quick Start

**User**: “Make onboarding better.”

**Response**:

1. Problem statement (draft): “Reduce new-user drop-off in the first 10 minutes from X% to Y%.”
2. Success criteria: define metric, timeframe, and measurement source.
3. Constraints: time, teams, platforms, risk tolerance.
4. Non-goals: “Not redesigning the whole product; focus on first-run.”
5. Next questions: current funnel, biggest drop step, top 3 user intents.

## Output Format

- **Problem statement**: 1–2 sentences, measurable if possible
- **Success criteria**: metrics + thresholds + timeframe
- **Constraints**: real limits (time, resources, laws/physics), plus assumed ones labeled
- **Non-goals**: explicit exclusions
- **Unknowns**: what must be learned next
- **Next questions**: smallest set to unblock progress

## Priority Hierarchy

1. **Clarity**: precise language, no hidden assumptions
2. **Simplicity**: minimal scope, minimal concepts
3. **Testability**: measurable success and falsifiable claims
