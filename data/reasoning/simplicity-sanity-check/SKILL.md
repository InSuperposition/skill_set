---
name: skill-reasoning-simplicity-sanity-check
description: Evaluates designs and plans for objective simplicity vs subjective ease, and recommends changes that reduce coupling and accidental complexity. Integrates first-principles validation with simplicity frameworks.
allowed-tools: Read
---

# Simplicity Sanity Check - Simple vs Easy

## Overview

This skill is a structural check applied to decisions, designs, and implementations. It helps you distinguish:

- **Simplicity** (objective): few parts, low coupling, orthogonal components
- **Ease** (subjective): familiar, convenient, fast-to-implement

It uses first principles to validate requirements and simplicity principles to minimize entanglement.

## Core Philosophy

- Choose simplicity first; add ease through tooling later.
- Complexity debt compounds; pay it down early.
- Prefer explicit over implicit, composition over inheritance, data visibility over magic.

## Core Capabilities

- Identify coupling and entanglement across concerns.
- Count concepts, abstractions, and dependency edges.
- Spot “easy now, complex later” tradeoffs.
- Recommend simplifications: remove features, remove indirection, make data inspectable, split responsibilities.

## When to Use

- Architecture proposals and migrations.
- Refactors that add abstraction layers.
- Tooling choices justified by convenience.

## Quick Start

**User**: “Let’s add a framework to speed up development.”

**Response**:

- Validate requirements: what must the framework enable?
- Simplicity check: what new concepts and couplings appear?
- Alternative: keep a simple core; add small utilities to improve ergonomics.

## Output Format

- **What’s simple vs what’s easy**: highlight mismatches
- **Complexity hotspots**: coupling, hidden state, implicit dependencies
- **Simplification actions**: 3–7 concrete moves
- **Risk callout**: where complexity debt will compound

## Priority Hierarchy

1. **Simplicity**: reduce entanglement and hidden coupling
2. **Clarity**: make data and dependencies visible
3. **Practicality**: keep recommendations implementable
