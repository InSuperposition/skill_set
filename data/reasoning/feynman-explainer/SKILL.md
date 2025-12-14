---
name: skill-reasoning-feynman-explainer
description: Uses the Feynman technique to explain ideas plainly, exposing gaps and unnecessary complexity. Pairs first-principles understanding with simplicity-first structure.
allowed-tools: Read
---

# Feynman Explainer - Explain It Like It’s Simple

## Overview

This skill tests understanding by forcing a plain-language explanation. If an explanation requires jargon or hand-waving, the concept likely isn’t understood or the design is entangled.

It combines first-principles reasoning (fundamentals first) with simplicity (few concepts, explicit dependencies).

## Core Philosophy

- If you can’t explain it simply, you don’t understand it yet.
- Jargon is allowed only when it compresses well-understood ideas.
- A simple explanation should correspond to an objectively simple structure.

## Core Capabilities

- Rewrite explanations for a non-expert audience without losing correctness.
- Identify where definitions are missing or circular.
- Expose hidden dependencies and implicit assumptions.
- Produce a “concept inventory” (the minimal set of ideas required).

## When to Use

- Writing docs, RFCs, decision records.
- Training new team members or onboarding.
- Architecture has become “magic” and hard to debug.

## Quick Start

**User**: “Explain why we need event sourcing.”

**Response**:

- Plain explanation: “We record every change as an append-only fact so we can rebuild state and audit history.”
- Concept inventory: events, immutability, projections, replay.
- Gap check: what problem demands audit + replay vs simpler DB history?
- Simplicity check: what coupling does event sourcing add?

## Output Format

- **Plain explanation**: 5–10 sentences, minimal jargon
- **Definitions**: any necessary terms
- **Concept inventory**: minimal list of concepts/components
- **Gap list**: where understanding is incomplete or contradictory
- **Simplification**: what can be removed or made explicit

## Priority Hierarchy

1. **Clarity**: plain language and explicit definitions
2. **Correctness**: do not oversimplify into falsehood
3. **Simplicity**: minimize concepts and hidden coupling
