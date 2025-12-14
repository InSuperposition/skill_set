---
name: skill-reasoning-counterexample-hunter
description: Finds counterexamples, edge cases, and failure modes that falsify claims or break designs. Uses first-principles falsification and simplicity to strengthen reasoning without adding unnecessary complexity.
allowed-tools: Read
---

# Counterexample Hunter - Try to Prove Yourself Wrong

## Overview

This skill improves reasoning by actively searching for counterexamples. It is useful for validating general claims (“this always works”) and for testing designs against edge cases. It combines:

- **First principles**: falsification and logical consistency.
- **Simplicity**: prefer robust simple rules over fragile exception-heavy ones.

## Core Philosophy

- One counterexample defeats an “always” claim.
- If a design needs many special cases, its structure is likely wrong.
- Find failures early, when fixes are cheapest.

## Core Capabilities

- Identify “universal” claims and convert them into falsifiable statements.
- Generate edge cases by varying inputs, environment, timing, scale, and adversaries.
- Test invariants and boundaries (nulls, empties, extremes, concurrency).
- Propose design adjustments that reduce special casing.

## When to Use

- You’re about to ship a critical change.
- An argument sounds too confident.
- You need to harden an API, algorithm, or process.

## Quick Start

**User**: “This permission check is safe.”

**Response**:

- What’s the strongest claim? “No unauthorized user can access resource R.”
- Counterexamples: stale cache, TOCTOU, role changes mid-request, missing tenant scoping.
- Suggest simplest mitigation: centralize authorization, make tenant ID explicit, add tests.

## Output Format

- **Claim**: the statement being tested
- **Potential counterexamples**: 5–15, grouped by category
- **Most plausible failures**: top 3 to validate first
- **Mitigations**: simplest structural changes that remove the class of failure

## Priority Hierarchy

1. **Correctness**: find real failure modes
2. **Rigor**: counterexamples tied to mechanisms, not vibes
3. **Simplicity**: reduce special-case logic by improving structure
