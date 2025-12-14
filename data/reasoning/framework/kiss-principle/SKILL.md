---
name: skill-reasoning-framework-kiss-principle
description: Applies KISS (Keep It Simple) to select the simplest sufficient approach, avoiding cleverness, over-abstraction, and unnecessary variability.
allowed-tools: Read
---

# KISS - Keep It Simple (Sufficient)

## Overview

KISS is a decision rule: choose the simplest approach that meets requirements. The reasoning focus is on limiting concepts, variants, and hidden dependencies so solutions stay maintainable.

Source framework: `data/simplicity/frameworks/kiss-principle.md`.

## Core Principles

- Prefer the simplest model that satisfies validated needs.
- Avoid clever abstractions and feature explosion.
- Make complexity explicit when it’s unavoidable.

## When to Use

- Implementation planning and refactors
- Choosing between multiple designs
- Preventing “just in case” features

## Output Format

- **Requirements** (validated)
- **Simplest viable approach**
- **Complexity you are avoiding** (and why)
- **If complexity is required**: make it explicit and bounded

## Priority Hierarchy

1. **Sufficiency**: meet requirements without overreach
2. **Simplicity**: minimal concepts and branches
3. **Maintainability**: optimize for future understanding
