---
name: skill-reasoning-framework-five-whys
description: Uses the Five Whys to trace symptoms to actionable root causes with an evidence-first, simplicity-first corrective action plan.
allowed-tools: Read
---

# Five Whys - Root Cause From a Concrete Symptom

## Overview

The Five Whys is an iterative questioning technique for drilling from an observable symptom to an actionable root cause. It is best when you have a concrete failure and need a fast, structured path to causality.

Source framework: `data/first-principles/frameworks/five-whys.md`.

## Core Principles

- Start with a **specific, observable problem statement**.
- Each “why” should produce a **cause**, not a restatement.
- Prefer **system/process mechanisms** over blame.
- Stop when you reach a cause that is **actionable** and prevents recurrence within scope.
- Keep the fix **simple**: smallest change that removes the causal mechanism.

## When to Use

- Incident and outage analysis
- Recurring bugs and regressions
- Process breakdowns (deployments, reviews, handoffs)

## When Not to Use

- Problems with many simultaneous contributing factors (use causal graphing)
- When you can’t answer “why” with evidence (collect facts first)

## Operating Procedure

1. Write the symptom as a measurable statement (what/where/when).
2. Ask “Why?” and answer with the best supported cause.
3. Repeat 3–7 times (not necessarily exactly five).
4. Label each answer: **fact**, **assumption**, or **hypothesis**.
5. Propose fixes that address the root cause, plus validation signals.

## Output Format

- **Problem statement**
- **Why chain** (each step labeled fact/assumption/hypothesis)
- **Root cause candidate**
- **Fix options** (ranked simplest-first)
- **Validation** (how to verify prevention)

## Priority Hierarchy

1. **Evidence**: don’t elevate guesses to facts
2. **Causality**: mechanism-focused chain
3. **Simplicity**: smallest effective corrective action
