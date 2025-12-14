---
name: skill-code
description: Generalized code-quality skillset that unifies review, audit, security, performance, and refactoring into 5 operational modes. Produces clear findings, actionable fixes, and verification plans across correctness, maintainability, testability, and architecture.
allowed-tools: Read, Write, Grep, Glob, Bash, TodoWrite
---

# Code - Unified Code Quality Skillset

## Overview

This skill generalizes the specialized skills in `data/code/` into one cohesive “code” capability: understand code, evaluate it across the major quality dimensions, and propose (or implement) changes safely.

It is designed for situations where you want one entry point and a clear operating mode rather than picking a specialized skill up front.

## Requirements

- No external dependencies required
- Language-agnostic by default; adapts to your repo conventions and stack

## Operating Principles

- **Safety first**: prefer minimal, reversible changes; avoid broad refactors without explicit agreement.
- **Correctness + security first**: don’t trade security/correctness for style or micro-performance.
- **Make reasoning explicit**: findings include evidence, impact, and concrete fixes.
- **Keep scope tight**: focus on the user’s goal, the changed surface area, and the highest-risk paths.

## Operational Modes (5)

### 1) Review Mode

Purpose: review a change set (PR/commit/diff) across the main dimensions and produce severity-tagged findings with fixes.

Best when: you have a specific change and need “ship/no-ship” guidance.

### 2) Audit Mode

Purpose: assess a codebase or subsystem holistically, identify hotspots and technical debt, and produce a prioritized remediation plan.

Best when: you need a roadmap, not a point-in-time review.

### 3) Security Mode

Purpose: threat-model and identify vulnerabilities (OWASP/CWE patterns), misuse of authn/authz, crypto mistakes, injection risks, and unsafe data handling.

Best when: the change touches sensitive paths or you’re doing a pre-release security pass.

### 4) Performance Mode

Purpose: identify bottlenecks and propose optimizations grounded in measurement: algorithmic complexity, I/O, DB queries, memory, caching, and concurrency.

Best when: you have a latency/throughput goal or proven slowness.

### 5) Refactor Mode

Purpose: improve structure without changing behavior; reduce complexity, increase testability, and make future changes safer.

Best when: you want to pay down debt or prepare for a feature.

## Core Capabilities

- **Correctness analysis**: invariants, edge cases, error handling, data validation, type safety
- **Security analysis**: input/output boundaries, authn/authz, secrets, supply chain, unsafe deserialization
- **Maintainability**: readability, duplication, complexity, naming, cohesion/coupling
- **Testability**: test seams, dependency injection, determinism, coverage of critical paths
- **Architecture**: boundaries, layering, responsibility separation, API contracts, domain modeling
- **Performance**: complexity, query patterns, caching, allocation/memory behavior, concurrency hazards

## Standard Workflow

1. **Intake**
   - What is the goal (ship, diagnose, refactor, harden, speed up)?
   - What is the scope (files/paths/features)?
   - What constraints matter (timeline, risk tolerance, compatibility)?
2. **Surface the critical paths**
   - What can cause data loss, security breach, or outages?
3. **Analyze and produce findings**
   - Evidence-based, severity-tagged, with concrete next steps.
4. **Propose fixes (and optionally implement)**
   - Prefer the smallest change that resolves the root issue.
5. **Verify**
   - Tests, static checks, and targeted reproduction steps.

## Output Format (Default)

- **Summary**: what was reviewed and the main risk call
- **Findings** (ordered by severity)
  - `CRITICAL`: security vulnerabilities, data loss, production-breaking behavior
  - `HIGH`: correctness issues, significant reliability/perf risks, major maintainability hazards
  - `MEDIUM`: important but non-blocking issues; missing tests for risky changes
  - `LOW`: clarity, style, small improvements
- **Recommendations**: prioritized fix list with suggested owners/effort
- **Verification**: how to confirm the fixes (tests, commands, checks)

## Mode-Specific Output Additions

### Review Mode

- **Merge recommendation**: block / caution / approve
- **Patch suggestions**: smallest diffs that address top issues

### Audit Mode

- **Hotspots**: high-churn or high-risk components
- **Debt map**: categories + impact
- **Roadmap**: 30/60/90-day prioritized plan

### Security Mode

- **Threat model**: assets, trust boundaries, attacker capabilities
- **Exploit narratives**: how a bug is abused and its impact
- **Mitigations**: layered defenses + tests

### Performance Mode

- **Hypotheses**: what you think is slow and why
- **Measurement plan**: profiling/benchmarks/logging
- **Before/after targets**: latency/throughput/memory metrics

### Refactor Mode

- **Behavioral constraints**: what must not change
- **Step plan**: small, test-backed refactor steps
- **Risk controls**: feature flags, staging, incremental commits

## When to Use

Use `skill-code` when you want a single generalized entry point for:

- Reviewing a PR/commit
- Auditing a subsystem or codebase
- Hardening security-sensitive code
- Improving performance based on evidence
- Refactoring safely to reduce complexity and increase testability

## When Not to Use

- You need product/UX decisions more than code decisions
- You want fully automated enforcement (prefer CI tooling/policies)
- You can’t define scope or success criteria (frame the problem first)

## References

### Code Subskills

- `skill-code-reviewer`: `data/code/reviewer/SKILL.md`
- `skill-code-auditor`: `data/code/auditor/SKILL.md`
- `skill-code-security`: `data/code/security/SKILL.md`
- `skill-code-performance`: `data/code/performance/SKILL.md`
- `skill-code-refactor`: `data/code/refactor/SKILL.md`
- `skill-code-correctness`: `data/code/correctness/SKILL.md`
- `skill-code-maintainability`: `data/code/maintainability/SKILL.md`
- `skill-code-testability`: `data/code/testability/SKILL.md`
- `skill-code-architecture`: `data/code/architecture/SKILL.md`
- `skill-code-guardian`: `data/code/guardian/SKILL.md`
- `skill-code-mentor`: `data/code/mentor/SKILL.md`
