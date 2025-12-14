---
name: skill-reasoning-root-cause-tracing
description: Performs evidence-based root-cause analysis using Five Whys, causal chains, and constraint checks. Emphasizes first-principles rigor and simplicity to avoid overfitting and overly complex fixes.
allowed-tools: Read
---

# Root Cause Tracing - From Symptom to Cause

## Overview

This skill finds root causes without drifting into speculation. It uses:

- **First principles**: step-by-step causal reasoning, facts vs assumptions.
- **Simplicity**: prefer the smallest fix that removes the causal mechanism, not a sprawling redesign.

## Core Philosophy

- “Root cause” means a cause that, if removed, prevents recurrence within scope.
- Avoid “cause inflation”: don’t add extra theories without evidence.
- Fixes should reduce future complexity, not add new coupling.

## Core Capabilities

- Run an evidence-based Five Whys flow.
- Build a causal chain: event → mechanism → enabling conditions.
- Separate human/process causes from system/technical causes without blame.
- Propose corrective actions and validations (tests, monitors, runbooks).
- Validate that the proposed fix addresses the mechanism, not just the symptom.

## When to Use

- Incidents, outages, production bugs, regressions.
- Repeated failures with “unknown reason”.
- Process breakdowns and operational issues.

## Quick Start

**User**: “Deployments keep failing.”

**Response**:

- Symptom: deployment fails at step X (fact).
- Why 1: step X fails due to timeout (fact).
- Why 2: timeout because artifact upload is slow (fact/measure).
- Why 3: slow because antivirus scan runs serially (fact).
- Root cause: serial scan + missing caching.
- Fix: cache scan results per artifact hash; add monitor for step duration.

## Output Format

- **Symptom**: what happened (measured)
- **Causal chain**: 3–7 links, each labeled fact/assumption
- **Root cause candidate**: minimal cause you can act on
- **Fix options**: smallest effective change first
- **Validation**: how you’ll know the fix worked

## Priority Hierarchy

1. **Evidence**: facts over speculation
2. **Causality**: mechanism-focused explanations
3. **Simplicity**: smallest change that prevents recurrence
