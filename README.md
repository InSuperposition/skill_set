# InSuperposition Skills Marketplace

> A curated collection of AI agent skills for code quality, Git operations, and structured reasoning

**Version:** 0.0.1
**Owner:** Forrest Galloway
**Repository:** [github.com/InSuperposition/skill_set](https://github.com/InSuperposition/skill_set)

## Overview

The InSuperposition Skills Marketplace is a comprehensive collection of specialized AI agent skills organized into four unified categories. Each skill is designed with a consistent philosophy of **safety-first**, **evidence-based**, and **simplicity-driven** approaches.

### Key Features

- **Specialized skills** across code quality, Git operations, and reasoning frameworks
- **Zero external dependencies** - works across all tech stacks and languages
- **Operational modes** per main skill for flexible workflow integration
- **Modular architecture** - use unified skills as entry points or specialized subskills for depth
- **Safety protocols** - minimal, reversible changes with explicit verification steps

### Philosophy

All skills in this marketplace share core operating principles:

- **Safety first** - Prefer minimal, reversible changes; avoid broad refactors without explicit agreement
- **Evidence-based** - Make reasoning explicit with findings, evidence, impact, and concrete fixes
- **Simplicity-driven** - Reduce complexity and coupling; choose the smallest viable structure
- **Language-agnostic** - Adapt to your repository conventions and technology stack

## Installation

### As a Claude Code Marketplace

```sh
# Clone the repository
git clone https://github.com/InSuperposition/skillset.git

# Add to Claude Code as a marketplace
# (Follow Claude Code marketplace integration instructions)
```

### Manual Usage

```sh
# Clone the repository
git clone https://github.com/InSuperposition/skillset.git

# Reference skill files directly from data/ directories
# Each skill is defined in a SKILL.md file
```

## The Core Skills

### 1. Code - Unified Code Quality Skillset

**[skill-code](data/code/SKILL.md)** - Generalized code-quality skillset that unifies review, audit, security, performance, and refactoring into one cohesive capability.

**Core Capabilities:**

- Correctness analysis (invariants, edge cases, error handling, type safety)
- Security analysis (input/output boundaries, authn/authz, secrets management)
- Maintainability (readability, duplication, complexity, naming)
- Testability (test seams, dependency injection, coverage of critical paths)
- Architecture (boundaries, layering, responsibility separation)
- Performance (complexity, query patterns, caching, memory behavior)

### Code Review Workflow

```md
Use skill-code in Review mode to evaluate a pull request:

1. Read the PR diff and changed files
2. Analyze across correctness, security, maintainability, testability, architecture
3. Produce severity-tagged findings (CRITICAL, HIGH, MEDIUM, LOW)
4. Provide merge recommendation (block / caution / approve)
5. Suggest minimal patches that address top issues
```

### 2. Git - Unified Git Skillset

**[skill-git](data/git/SKILL.md)** - Generalized Git skillset that unifies advisory guidance, safe execution, workflow specialization, and recovery.

**Core Capabilities:**

- Workflow specialization (worktrees, submodules, rebasing, notes)
- Safety protocols and decision discipline for risky operations
- Troubleshooting and recovery using reflog and backups
- Repository maintenance via health checks and configuration

#### Git Operation Workflow

```md
Use skill-git in Worker mode for safe Git operations:

1. State goal and scope (e.g., "rebase feature branch on main")
2. Pre-flight checks: status, branch state, remote state
3. Create backup (tag or branch) before risky operations
4. Execute smallest safe sequence with verification
5. Confirm success and provide recovery instructions if needed
```

### 3. Reasoning - Unified Thinking Skillset

**[skill-reasoning](data/reasoning/SKILL.md)** - Generalized reasoning skillset with core operational modes plus framework modes. Turns ambiguity into clarity and produces decisions with explicit rationale.

**Operating Principles:**

- **Facts before stories** - Label observations vs interpretations vs assumptions
- **Make constraints explicit** - Distinguish real constraints from assumed constraints
- **Prefer falsifiable claims** - Turn beliefs into tests and predictions
- **Simplicity first** - Reduce coupling and complexity debt

#### Problem-Solving Workflow

```md
Use skill-reasoning through the 5-mode workflow:

1. Frame: Define problem, success criteria, constraints
2. Challenge: Surface assumptions, find counterexamples
3. Map: Visualize dependencies, find seams and leverage points
4. Build: Generate options from fundamentals, design experiments
5. Decide: Synthesize into recommendation with validation plan
```

### 4. JavaScript - Unified JavaScript & TypeScript Mastery Skillset

**[skill-javascript](data/javascript/SKILL.md)** - Unified JavaScript and TypeScript mastery skillset covering language features (ES2015+), performance optimization (V8 internals), type safety (TypeScript), functional programming, and design patterns.

**Core Capabilities:**

- Language mastery (ES2015+ features, async/await, modules, closures, iterators, generators)
- Performance optimization (V8 internals, JIT compilation, memory management, algorithmic analysis)
- Type safety (advanced TypeScript types, generics, utility types, type guards)
- Functional programming (pure functions, immutability, composition, higher-order functions)
- Design patterns (GoF patterns, module patterns, architectural solutions)

**Operating Principles:**

- **Modern standards first** - Prioritize ES2015+ features over legacy patterns
- **Type safety as enabler** - Types prevent bugs without sacrificing flexibility
- **Performance through understanding** - Optimize based on V8 internals and measurements
- **Functional when appropriate** - Immutability and purity improve reliability
- **Patterns solve problems** - Apply patterns when they reduce complexity

#### JavaScript Development Workflow

```md
Use skill-javascript across 5 operational modes:

1. Language Mode: Modernize code with ES2015+, master async patterns
2. Performance Mode: Optimize V8 performance, analyze complexity
3. TypeScript Mode: Add type safety, design type architectures
4. Functional Mode: Apply functional patterns, ensure immutability
5. Patterns Mode: Implement design patterns, structure architecture
```

## Contributing

### Skill Format

All skills follow a consistent markdown format:

```markdown
# Skill Name

## Overview
[Description of what the skill does]

## Requirements
[Any dependencies or prerequisites]

## Operating Principles
[Core principles that guide the skill]

## Operational Modes (for unified skills)
[Description of the different modes of operation]

## Core Capabilities
[Key capabilities and features]

## Standard Workflow
[Step-by-step workflow]

## [Additional sections as needed]
```

### Directory Structure

Skills are organized hierarchically:

```sh
data/
  code/
    SKILL.md                    # Main unified skill
    reviewer/SKILL.md           # Specialized subskill
    auditor/SKILL.md
    ...
  git/
    SKILL.md                    # Main unified skill
    guide/SKILL.md              # Specialized subskill
    ...
  javascript/
    SKILL.md                    # Main unified skill
    language/SKILL.md           # Specialized subskill
    performance/SKILL.md
    typescript/SKILL.md
    functional/SKILL.md
    patterns/SKILL.md
  reasoning/
    SKILL.md                    # Main unified skill
    problem-framing/SKILL.md    # Specialized subskill
    framework/
      five-whys/SKILL.md        # Framework subskill
      ...
    ...
```

### Adding New Skills

1. Create a new directory under the appropriate category (`data/code/`, `data/git/`, `data/javascript/`, `data/reasoning/`)
2. Add a `SKILL.md` file following the standard markdown format
3. Follow the established format and operating principles
4. Update the parent category's SKILL.md to reference the new subskill
5. Submit a pull request with your new skill

## License

See repository for license information.
