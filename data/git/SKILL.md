---
name: skill-git
description: Generalized Git skillset that unifies advisory guidance, safe execution, workflow specialization, and recovery. Provides 5 operational modes and references to the git subskills in this repo.
allowed-tools: Read, Write, Grep, Glob, Bash, TodoWrite
---

# Git - Unified Git Skillset

## Overview

This skill is the generalized entry point for Git work in this repo. It provides a consistent safety-first approach to planning, executing, and recovering from Git operations, while delegating depth to the specialized subskills under `data/git/`.

Use it when you want one “Git” skill with clear operational modes, and a built-in reference to the relevant subskills.

## Requirements

- Git v2.5.0+

## Operating Principles

- **Safety first**: avoid data loss and team disruption; create backups before risky changes.
- **Make intent explicit**: state goals, scope, and constraints before running commands.
- **Prefer reversible moves**: merges/reverts over rewriting shared history unless explicitly coordinated.
- **Verify outcomes**: confirm state after each meaningful operation.

## Operational Modes (5)

### 1) Guide Mode

Purpose: explain concepts, recommend strategies, and provide copy-paste ready commands without executing.

Primary subskill: `skill-git-guide`.

### 2) Worker Mode

Purpose: execute git operations with pre-flight checks, confirmations, backups, and post-verification.

Primary subskill: `skill-git-worker`.

### 3) Safety Mode

Purpose: classify risk, enforce protected-branch discipline, and apply pre-flight + backup protocols for any operation that can lose data or rewrite history.

Primary subskill: `skill-git-safety`.

### 4) Recovery Mode

Purpose: recover from mistakes using reflog and safe recovery playbooks (reset/rebase/amend/branch deletion).

Primary subskill: `skill-git-recovery`.

### 5) Advanced Mode

Purpose: on-demand use of advanced git capabilities (sparse checkout, partial clone, LFS, hooks, bisect, filter-repo) when justified by constraints.

Primary subskill: `skill-git-advanced`.

## Core Capabilities

- Workflow specialization across worktrees, submodules, rebasing, and notes
- Safety protocols and decision discipline for risky operations
- Troubleshooting and recovery using reflog and backups
- Repo maintenance via health checks and configuration recommendations

## Standard Workflow

1. **Intake**: goal, scope, risk tolerance, and collaboration context.
2. **Choose mode**: guide vs worker; safety/recovery/advanced when relevant.
3. **Pre-flight**: status, branch/upstream, remote state, and constraints.
4. **Execute (if requested)**: smallest safe sequence with verification.
5. **Recover (if needed)**: reflog-first, create recovery branches, verify.

## Output Format (Default)

- **Goal** and **current state**
- **Recommended mode**
- **Plan** (steps + commands)
- **Risks + safeguards** (backups, confirmations, protected branches)
- **Verification** (checks to confirm success)
- **Recovery** (how to undo if something goes wrong)

## References

### Git Subskills

- `skill-git-guide`: `data/git/guide/SKILL.md`
- `skill-git-worker`: `data/git/worker/SKILL.md`
- `skill-git-safety`: `data/git/safety/SKILL.md`
- `skill-git-recovery`: `data/git/recovery/SKILL.md`
- `skill-git-rebase`: `data/git/rebase/SKILL.md`
- `skill-git-worktree`: `data/git/worktree/SKILL.md`
- `skill-git-submodule`: `data/git/submodule/SKILL.md`
- `skill-git-notes`: `data/git/notes/SKILL.md`
- `skill-git-health`: `data/git/health/SKILL.md`
- `skill-git-config`: `data/git/config/SKILL.md`
- `skill-git-advanced`: `data/git/advanced/SKILL.md`
