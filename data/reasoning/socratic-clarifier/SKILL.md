---
name: skill-reasoning-socratic-clarifier
description: Uses Socratic questioning to clarify definitions, surface hidden assumptions, and tighten arguments. Combines first-principles inquiry with simplicity to converge on crisp, actionable statements.
allowed-tools: Read
---

# Socratic Clarifier - Ask the Right Questions

## Overview

This skill applies structured Socratic questioning to turn fuzzy statements into precise, testable claims. It supports first principles by challenging assumptions and supports simplicity by reducing conceptual clutter.

## Core Philosophy

- Precision is kindness: vague words hide disagreement.
- Questions should narrow uncertainty, not expand debate.
- Prefer fewer, sharper questions over many shallow ones.

## Core Capabilities

- Clarify meanings: “What do you mean by X?”
- Probe evidence: “What supports that claim?”
- Expose assumptions: “What must be true for this to work?”
- Explore implications: “If this is true, what follows?”
- Find better questions: “What are we actually trying to decide?”

## When to Use

- Stakeholders disagree but can’t articulate why.
- Requirements contain ambiguous terms (“fast”, “secure”, “simple”).
- Arguments rely on authority or analogy without grounding.

## Quick Start

**User**: “This architecture is too complex.”

**Response**:

- What does “complex” mean here: too many concepts, too much coupling, too hard to change?
- Which change is currently hard, specifically?
- What evidence shows this complexity is harming outcomes?
- What simpler structure would still satisfy constraints?

## Output Format

- **Clarifying questions**: 5–12, ordered to reduce ambiguity fastest
- **Refined statement**: a rewritten, precise version of the original claim
- **Key assumptions**: the top assumptions revealed by questioning
- **Next step**: smallest action to answer the most important question

## Priority Hierarchy

1. **Clarity**: precise definitions before conclusions
2. **Rigor**: assumptions and evidence made explicit
3. **Simplicity**: converge quickly; avoid question sprawl
