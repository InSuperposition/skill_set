# InSuperposition Skills Marketplace

> A curated collection of AI agent skills for code quality, Git operations, and structured reasoning

**Version:** 0.0.1
**Owner:** Forrest Galloway
**Repository:** [github.com/InSuperposition/skill_set](https://github.com/InSuperposition/skill_set)

## Overview

The InSuperposition Skills Marketplace is a comprehensive collection of specialized AI agent skills organized into three unified categories. Each skill is designed with a consistent philosophy of **safety-first**, **evidence-based**, and **simplicity-driven** approaches.

### Key Features

- **52+ specialized skills** across code quality, Git operations, and reasoning frameworks
- **Zero external dependencies** - works across all tech stacks and languages
- **5 operational modes** per main skill for flexible workflow integration
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

```bash
# Clone the repository
git clone https://github.com/InSuperposition/skillset.git

# Add to Claude Code as a marketplace
# (Follow Claude Code marketplace integration instructions)
```

### Manual Usage

```bash
# Clone the repository
git clone https://github.com/InSuperposition/skillset.git

# Reference skill files directly from data/ directories
# Each skill is defined in a SKILL.md file with YAML frontmatter
```

## The Three Core Skills

### 1. Code - Unified Code Quality Skillset

**[skill-code](data/code/SKILL.md)** - Generalized code-quality skillset that unifies review, audit, security, performance, and refactoring into one cohesive capability.

**Core Capabilities:**

- Correctness analysis (invariants, edge cases, error handling, type safety)
- Security analysis (input/output boundaries, authn/authz, secrets management)
- Maintainability (readability, duplication, complexity, naming)
- Testability (test seams, dependency injection, coverage of critical paths)
- Architecture (boundaries, layering, responsibility separation)
- Performance (complexity, query patterns, caching, memory behavior)

### 2. Git - Unified Git Skillset

**[skill-git](data/git/SKILL.md)** - Generalized Git skillset that unifies advisory guidance, safe execution, workflow specialization, and recovery.

**Core Capabilities:**

- Workflow specialization (worktrees, submodules, rebasing, notes)
- Safety protocols and decision discipline for risky operations
- Troubleshooting and recovery using reflog and backups
- Repository maintenance via health checks and configuration

### 3. Reasoning - Unified Thinking Skillset

**[skill-reasoning](data/reasoning/SKILL.md)** - Generalized reasoning skillset with 5 core operational modes plus framework modes. Turns ambiguity into clarity and produces decisions with explicit rationale.

**Operating Principles:**

- **Facts before stories** - Label observations vs interpretations vs assumptions
- **Make constraints explicit** - Distinguish real constraints from assumed constraints
- **Prefer falsifiable claims** - Turn beliefs into tests and predictions
- **Simplicity first** - Reduce coupling and complexity debt

## Usage Examples

### Code Review Workflow

```md
Use skill-code in Review mode to evaluate a pull request:

1. Read the PR diff and changed files
2. Analyze across correctness, security, maintainability, testability, architecture
3. Produce severity-tagged findings (CRITICAL, HIGH, MEDIUM, LOW)
4. Provide merge recommendation (block / caution / approve)
5. Suggest minimal patches that address top issues
```

### Git Operation Workflow

```md
Use skill-git in Worker mode for safe Git operations:

1. State goal and scope (e.g., "rebase feature branch on main")
2. Pre-flight checks: status, branch state, remote state
3. Create backup (tag or branch) before risky operations
4. Execute smallest safe sequence with verification
5. Confirm success and provide recovery instructions if needed
```

### Problem-Solving Workflow

```md
Use skill-reasoning through the 5-mode workflow:

1. Frame: Define problem, success criteria, constraints
2. Challenge: Surface assumptions, find counterexamples
3. Map: Visualize dependencies, find seams and leverage points
4. Build: Generate options from fundamentals, design experiments
5. Decide: Synthesize into recommendation with validation plan
```

## Contributing

### Skill Format

All skills follow a consistent format:

```markdown
---
name: skill-name
description: Brief description of the skill's purpose
allowed-tools: Read, Write, Grep, Glob, Bash, TodoWrite
---

# Skill Name

## Overview
[Description of what the skill does]

## Requirements
[Any dependencies or prerequisites]

## Operating Principles
[Core principles that guide the skill]

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
  reasoning/
    SKILL.md                    # Main unified skill
    problem-framing/SKILL.md    # Specialized subskill
    framework/
      five-whys/SKILL.md        # Framework subskill
      ...
    ...
```

### Adding New Skills

1. Create a new directory under the appropriate category (`data/code/`, `data/git/`, `data/reasoning/`)
2. Add a `SKILL.md` file with YAML frontmatter
3. Include required fields: `name`, `description`, `allowed-tools`
4. Follow the established format and operating principles
5. Update the parent category's SKILL.md to reference the new subskill
6. Submit a pull request with your new skill

### YAML Frontmatter Fields

Required fields:

- `name`: Skill identifier (e.g., `skill-code-reviewer`)
- `description`: One-sentence description of the skill's purpose
- `allowed-tools`: Comma-separated list of tools the skill can use

## Project Information

**Marketplace Name:** InSuperposition
**Owner:** Forrest Galloway ([f.galloway@gmail.com](mailto:f.galloway@gmail.com))
**Version:** 0.0.1
**Repository:** [https://github.com/InSuperposition/skillset](https://github.com/InSuperposition/skillset)
**Homepage:** [https://github.com/InSuperposition/skillset](https://github.com/InSuperposition/skillset)

## License

See repository for license information.

---

Built with care for safety-first, evidence-based, and simplicity-driven AI agent skills.
