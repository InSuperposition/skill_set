# JavaScript - Unified JavaScript & TypeScript Mastery Skillset

## Overview

This skill generalizes JavaScript and TypeScript expertise into one cohesive capability: master the language, optimize performance, ensure type safety, apply functional principles, and leverage proven patterns.

It is designed for situations where you want one entry point and a clear operating mode for JavaScript/TypeScript mastery rather than picking a specialized skill up front.

Focus: Core JavaScript/TypeScript language and runtime - no framework-specific guidance (React, Vue, Express, etc.).

## Requirements

- No external dependencies required
- Works with any JavaScript/TypeScript codebase
- Language-agnostic by default; adapts to your conventions

## Operating Principles

- **Modern standards first**: prioritize ES2015+ features over legacy patterns; future-proof code.
- **Type safety as enabler**: types prevent bugs without sacrificing flexibility; use TypeScript strategically.
- **Performance through understanding**: optimize based on V8 internals and measurements, not myths or cargo-cult patterns.
- **Functional when appropriate**: immutability and purity improve reliability and testability; apply where it adds value.
- **Patterns solve problems**: apply design patterns when they reduce complexity, not for their own sake.

## Operational Modes (5)

### 1) Language Mode

**Purpose**: Master modern JavaScript language features (ES2015+) and core semantics.

**Best when**: You need to modernize code, understand async patterns, implement advanced features (Proxies, Iterators, Generators), or make module architecture decisions.

**Subskill**: `skill-javascript-language` (see `data/javascript/language/SKILL.md`)

### 2) Performance Mode

**Purpose**: Optimize runtime performance through V8 understanding, algorithmic analysis, and memory management.

**Best when**: Your code is slow or memory-intensive, you're investigating V8 deoptimizations, optimizing hot paths, or making algorithm/data structure selection decisions.

**Subskill**: `skill-javascript-performance` (see `data/javascript/performance/SKILL.md`)

### 3) TypeScript Mode

**Purpose**: Design type-safe systems with advanced TypeScript patterns and type-level programming.

**Best when**: You need type-safe API design, complex generic implementations, TypeScript migration planning, or type system architecture decisions.

**Subskill**: `skill-javascript-typescript` (see `data/javascript/typescript/SKILL.md`)

### 4) Functional Mode

**Purpose**: Apply functional programming principles for predictable, composable, and testable code.

**Best when**: You want predictable code, complex data transformations, immutability guarantees, or to reduce bugs through functional patterns.

**Subskill**: `skill-javascript-functional` (see `data/javascript/functional/SKILL.md`)

### 5) Patterns Mode

**Purpose**: Implement proven design patterns and architectural solutions for JavaScript/TypeScript.

**Best when**: You're making architectural decisions, recognizing opportunities to apply known patterns, organizing code structure, or designing state management systems.

**Subskill**: `skill-javascript-patterns` (see `data/javascript/patterns/SKILL.md`)

## Core Capabilities

- **Language Mastery**: ES2015+ features, async/await, Promises, event loop, modules, closures, Proxies, Symbols, Iterators, Generators
- **Performance Optimization**: V8 internals, JIT compilation, hidden classes, GC behavior, memory leak detection, Big O analysis, event loop optimization
- **Type Safety**: Advanced types, generics, utility types, type guards, conditional/mapped types, template literal types, migration strategies
- **Functional Programming**: Pure functions, immutability, composition, higher-order functions, currying, functors, monads
- **Design Patterns**: GoF patterns (Singleton, Factory, Observer), module patterns, dependency injection, event-driven architecture, state management

## Standard Workflow

1. **Understand Context**
   - What is the goal (modernization, optimization, type safety, refactoring, architecture)?
   - What is the scope (files/modules/features)?
   - What constraints matter (compatibility, performance targets, migration timeline)?

2. **Identify Mode(s)**
   - Which mode(s) best address the core problem?
   - Are multiple modes needed (e.g., Language + TypeScript for typed modern code)?

3. **Analyze Code**
   - Surface issues, patterns, and opportunities
   - Provide evidence-based findings with severity/priority

4. **Recommend Solutions**
   - Specific, actionable recommendations with code examples
   - Explain tradeoffs and rationale
   - Provide migration paths for refactoring

5. **Verify Impact**
   - How to measure success (tests, benchmarks, type checking, bundle size)
   - Validation steps and rollback options

## Output Format (Default)

- **Context**: What was analyzed and why
- **Findings**: Issues or opportunities identified (severity/priority-tagged)
  - `CRITICAL`: Security vulnerabilities, data corruption, production-breaking
  - `HIGH`: Correctness issues, significant performance problems, major type safety gaps
  - `MEDIUM`: Important improvements, missing patterns, moderate inefficiencies
  - `LOW`: Style, clarity, small optimizations
- **Recommendations**: Specific solutions with code examples
- **Migration Path**: Step-by-step approach for refactoring (when applicable)
- **Verification**: How to confirm improvements (tests, benchmarks, type checks)

## Mode Selection Guide

### Use Language Mode when

- Understanding or modernizing JavaScript syntax
- Working with async/await, Promises, or event loop
- Implementing iterators, generators, proxies, symbols
- Module system questions (ESM, CommonJS, dynamic imports)
- Scope, closures, and execution context issues

### Use Performance Mode when

- Code is slow or memory-intensive
- Need V8 optimization understanding (JIT, hidden classes, inline caching)
- Algorithmic complexity analysis (Big O)
- Bundle size concerns or code splitting
- Memory leak investigation
- Event loop blocking detection

### Use TypeScript Mode when

- Adding type safety to codebase
- Complex generic type problems
- TypeScript migration planning
- Type system architecture decisions
- Type error debugging or inference issues
- Creating reusable typed libraries

### Use Functional Mode when

- Need predictable, testable code
- Complex data transformations
- Immutability requirements
- Reducing bugs through functional patterns
- Composition over inheritance
- Mathematical/algorithmic problems

### Use Patterns Mode when

- Architectural decisions needed
- Recognizing design pattern opportunities
- Code organization challenges
- Refactoring to proven solutions
- State management design
- Event-driven system design
- Error handling architecture

## Mode Integration Examples

The modes are designed to complement each other:

**Example 1: Optimizing a data transformation pipeline**

- **Language Mode**: Use modern array methods (map, filter, reduce)
- **Performance Mode**: Analyze if array methods cause performance issues at scale
- **TypeScript Mode**: Add type safety to transformation functions
- **Functional Mode**: Ensure transformations are pure and composable
- **Patterns Mode**: Apply Pipeline pattern for clarity

**Example 2: Building a state management system**

- **Language Mode**: Use Proxy for reactive state tracking
- **Performance Mode**: Optimize state updates to minimize overhead
- **TypeScript Mode**: Type state shape and actions
- **Functional Mode**: Immutable state updates
- **Patterns Mode**: Observer pattern for subscriptions, Command pattern for actions

**Example 3: Migrating legacy code to modern TypeScript**

- **Language Mode**: Replace callbacks with async/await, use modules instead of IIFEs
- **TypeScript Mode**: Add types incrementally, use utility types
- **Functional Mode**: Refactor to pure functions where applicable
- **Patterns Mode**: Identify and formalize existing patterns
- **Performance Mode**: Ensure modernization doesn't degrade performance

## When to Use

Use `skill-javascript` when you want a single entry point for:

- Modernizing JavaScript code to ES2015+
- Optimizing JavaScript runtime performance
- Adding TypeScript and type safety
- Applying functional programming principles
- Implementing design patterns and architectural solutions
- Any combination of the above

## When Not to Use

- Framework-specific questions (React, Vue, Angular, Express, etc.) - seek framework-specific guidance
- Build tooling configuration (webpack, vite, rollup) - seek tooling-specific guidance
- Node.js-specific APIs or runtime features - seek Node.js-specific guidance
- Browser APIs (DOM, WebGL, WebRTC) - seek browser API-specific guidance
- You can't define scope or success criteria - frame the problem first

## References

### JavaScript Subskills

- `skill-javascript-language`: `data/javascript/language/SKILL.md`
- `skill-javascript-performance`: `data/javascript/performance/SKILL.md`
- `skill-javascript-typescript`: `data/javascript/typescript/SKILL.md`
- `skill-javascript-functional`: `data/javascript/functional/SKILL.md`
- `skill-javascript-patterns`: `data/javascript/patterns/SKILL.md`
