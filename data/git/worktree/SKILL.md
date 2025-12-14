---
name: skill-git-worktree
description: Master-level expertise in git worktrees for managing multiple working directories from a single repository. Specialized in parallel feature development, hotfix workflows, code review patterns, and worktree-submodule integration.
allowed-tools: Read
---
# Git Worktree - Multiple Working Directories

## Overview

Git worktrees enable **multiple working directories** attached to a single repository, allowing simultaneous work on different branches without context switching. This is one of git's most powerful yet underutilized features, enabling workflows that would be cumbersome or impossible with traditional branch switching.

Instead of switching branches in a single directory (losing your context each time), worktrees let you maintain separate directories for each branch - each with its own working state, uncommitted changes, and IDE configuration, while sharing the same git repository, commit history, and object database.

Worktrees excel at parallel feature development, emergency hotfix workflows, code review in isolation, CI/testing environments, and multi-version maintenance. Understanding worktrees transforms how you manage complex development workflows.

## Requirements

- Git v2.5.0+ (worktrees introduced July 2015)
- Disk space for multiple working directories
- Understanding of git branches and working directory concepts

## Core Capabilities

- **Parallel Branch Work** - Work on multiple branches simultaneously without switching contexts
- **Shared Repository** - All worktrees share the same .git directory, refs, and object database
- **Independent Working State** - Each worktree has separate working directory, index, and uncommitted changes
- **Branch Isolation** - Each worktree checks out a different branch (enforced by git)
- **Worktree Lifecycle** - Create, list, move, repair, and remove worktrees
- **Sparse Checkout Integration** - Combine worktrees with sparse checkout for large repositories
- **Submodule Coordination** - Handle per-worktree submodule initialization and state
- **Performance Optimization** - Disk space savings compared to multiple clones

## When to Use

**Use git-worktree for**:

- Parallel feature development without context switching or stashing
- Emergency hotfixes while preserving current work state
- Code review in isolated environment without affecting main work
- Running tests in clean environment during development
- Multi-version maintenance (supporting multiple release branches)
- Bisect operations while continuing feature development
- CI/CD workflows requiring parallel branch builds
- Comparing implementations side-by-side in separate directories

**Don't use git-worktree for**:

- Simple branch switching (use git checkout instead)
- Single-task workflows (worktrees add complexity without benefit)
- Shared team workspaces (each developer manages their own worktrees)
- When disk space is severely constrained

## Integration with Other Skills

- **first-principles**: Question whether multiple concurrent branches are necessary, consider simpler linear workflows
- **simplicity**: Prefer single-branch focus when possible, use worktrees only when parallelism provides clear value
- **git-worker**: Execute worktree creation, removal, and management with safety checks
- **git-submodule**: Coordinate worktree creation with submodule initialization
- **git-rebase**: Rebase in worktrees without affecting other working directories
- **git-recovery**: Each worktree has independent reflog for recovery
- **git-advanced**: Combine worktrees with sparse checkout for large repositories

## Core Concepts

### Traditional Git vs Worktree Workflow

**Traditional Git Workflow**:

```sh
my-repo/
├── .git/
├── src/
└── README.md

# To work on different branches:
git checkout feature-a    # Switch entire working directory
git checkout feature-b    # Switch again, lose feature-a context
git checkout main        # Switch back
```

Problem: Only one branch at a time, requires stashing or committing incomplete work.

**Worktree Workflow**:

```sh
my-repo/              (main working directory - main branch)
├── .git/
├── src/
└── README.md

../feature-a/         (worktree - feature-a branch)
├── .git             (file pointing to my-repo/.git)
├── src/
└── README.md

../feature-b/         (worktree - feature-b branch)
├── .git
├── src/
└── README.md
```

Benefit: Work on multiple branches simultaneously, each with independent state.

### Shared Repository Architecture

**What's Shared**:

- Same .git directory (all worktrees reference main repository)
- Same refs (branches, tags, commits)
- Same object database (commits, trees, blobs)
- Same configuration and hooks

**What's Independent**:

- Working directory contents
- Index (staging area)
- Uncommitted changes
- Sparse checkout patterns (if configured)
- Submodule state

### Branch Rules

- Each worktree checks out a **different branch** (enforced by git)
- Cannot checkout same branch in multiple worktrees simultaneously
- Detached HEAD is allowed in multiple worktrees
- Creating new branch requires different name per worktree

## Basic Operations

### Creating Worktrees

**Create worktree from existing branch**:

```bash
git worktree add ../feature-auth feature/auth
# Creates worktree at ../feature-auth, checks out feature/auth
```

**Create worktree with new branch**:

```bash
git worktree add ../feature-new -b feature/new-feature
# Creates new branch feature/new-feature and worktree
```

**Create worktree from remote branch**:

```bash
git worktree add ../hotfix origin/hotfix/urgent-fix
# Creates worktree tracking remote branch
```

**Create worktree in detached HEAD**:

```bash
git worktree add ../review abc1234
# Creates worktree at specific commit (detached HEAD)
```

### Listing Worktrees

```bash
# Basic list
git worktree list
# Output:
# /path/to/main-repo    abc1234 [main]
# /path/to/feature-a    def5678 [feature/auth]

# Detailed information (porcelain format)
git worktree list --porcelain
```

### Removing Worktrees

**Safe removal** (worktree must be clean):

```bash
git worktree remove ../feature-auth
# Fails if uncommitted changes exist
```

**Force removal** (discard changes):

```bash
git worktree remove --force ../feature-auth
# Removes worktree even with uncommitted changes
```

**Remove deleted worktree reference**:

```bash
git worktree prune
# Clean up references to manually deleted worktrees
```

### Moving Worktrees

```bash
# Move the directory
mv ../feature-auth ../renamed-feature

# Update git's records
git worktree repair ../renamed-feature
```

## Workflow Patterns

### Pattern 1: Parallel Feature Development

**Use Case**: Work on multiple features simultaneously without context switching

```bash
# Setup
git worktree add ../feature-auth feature/auth
git worktree add ../feature-payments feature/payments

# Work in different terminals/IDEs
Terminal 1: cd ../feature-auth && code .
Terminal 2: cd ../feature-payments && code .

# Each has independent:
# - Uncommitted changes
# - IDE state and configuration
# - Running processes/servers

# Cleanup when features complete
git worktree remove ../feature-auth
git worktree remove ../feature-payments
```

**Benefits**:

- No stashing required when switching contexts
- Run tests in one worktree while coding in another
- Different IDE/editor state for each feature
- Compare implementations side-by-side

### Pattern 2: Hotfix Workflow

**Use Case**: Emergency fix needed while working on feature

```bash
# Currently working on feature branch with uncommitted changes
# Emergency issue reported!

# Create hotfix worktree from main
git worktree add ../hotfix -b hotfix/urgent-fix main

# Work on hotfix in separate directory
cd ../hotfix
# Fix issue, test, commit
git add .
git commit -m "Fix critical security issue"
git push origin hotfix/urgent-fix

# Return to feature work (completely unchanged!)
cd ../my-repo
# All uncommitted changes still present

# Cleanup after hotfix merged
git worktree remove ../hotfix
```

**Benefits**:

- Feature work remains completely untouched
- No stashing or committing incomplete work
- Hotfix gets full attention in clean environment
- Easy context switch back to feature

### Pattern 3: Code Review Workflow

**Use Case**: Review pull requests without affecting current work

```bash
# Fetch PR branch
git fetch origin pull/123/head:pr-123

# Create review worktree
git worktree add ../review-pr-123 pr-123

# Review in separate environment
cd ../review-pr-123
# Run tests, explore code, make notes
# Leave comments in review system

# Cleanup after review
cd -
git worktree remove ../review-pr-123
git branch -d pr-123
```

**Benefits**:

- Review code in complete isolation
- Run tests without affecting your work
- Make experimental changes safely
- Easy comparison with your current work

### Pattern 4: CI/Testing Workflow

**Use Case**: Run tests in clean environment while continuing development

```bash
# Create test worktree
git worktree add ../test-env main

# Run tests in background
cd ../test-env
make test &

# Continue development in main worktree
cd ../my-repo
# Keep working while tests run

# Check test results
cd ../test-env
# Review output

# Cleanup
cd ../my-repo
git worktree remove ../test-env
```

**Benefits**:

- Tests run in isolated environment
- Development continues uninterrupted
- Reproducible test conditions
- Multiple test configurations possible

### Pattern 5: Multi-Version Support

**Use Case**: Maintain multiple release versions simultaneously

```bash
# Main development
my-repo/          (main branch - v2.0 development)

# Support branches in separate worktrees
../v1.9-support/  (v1.9.x maintenance)
../v1.8-support/  (v1.8.x security fixes)

# Create support worktrees
git worktree add ../v1.9-support release/v1.9
git worktree add ../v1.8-support release/v1.8

# Apply security fix to all versions
cd ../v1.8-support
# Apply fix, test, commit, push

cd ../v1.9-support
# Apply fix, test, commit, push

cd ../my-repo
# Merge fix into main if applicable
```

**Benefits**:

- Simultaneous multi-version maintenance
- Easy cherry-picking between versions
- Separate build artifacts per version
- Clear version isolation

## Advanced Workflows

### Worktrees with Sparse Checkout

**Use Case**: Large repository - only checkout needed files in worktree

```bash
# Create worktree without checkout
git worktree add --no-checkout ../sparse-feature feature/big-repo-work
cd ../sparse-feature

# Enable sparse checkout
git sparse-checkout init --cone

# Specify directories to include
git sparse-checkout set src/auth tests/auth

# Checkout with only specified paths
git checkout feature/big-repo-work
```

**Benefits**:

- Faster worktree creation for large repos
- Reduced disk usage
- Focused development environment
- Each worktree can have different sparse patterns

### Different Sparse Patterns Per Worktree

```bash
# Main repo: Full checkout
my-repo/

# Frontend worktree: Only frontend code
git worktree add --no-checkout ../frontend-work feature/ui
cd ../frontend-work
git sparse-checkout set src/frontend tests/frontend
git checkout feature/ui

# Backend worktree: Only backend code
git worktree add --no-checkout ../backend-work feature/api
cd ../backend-work
git sparse-checkout set src/backend tests/backend
git checkout feature/api
```

### Worktrees for Bisect Operations

**Use Case**: Find regression while continuing development

```bash
# Start bisect in dedicated worktree
git worktree add ../bisect-session main
cd ../bisect-session

git bisect start
git bisect bad HEAD
git bisect good v1.0.0

# Git will checkout commits to test
# Test each commit
make test && git bisect good || git bisect bad

# Continue until regression found
# Meanwhile, main worktree remains on your feature branch

# Cleanup
cd ../my-repo
git worktree remove ../bisect-session
```

## Worktrees + Submodules

### Challenge: Submodule State Divergence

**Problem**: Worktrees share repository, but submodules are **per-worktree**.

```sh
my-repo/
├── .git/
├── submodule-a/      # Checked out to commit abc123
└── README.md

../feature-worktree/
├── .git -> ../my-repo/.git
├── submodule-a/      # Might be at different commit def456!
└── README.md
```

### Solution: Initialize Submodules in Each Worktree

```bash
# Create worktree
git worktree add ../feature-auth feature/auth

# Initialize submodules in new worktree
cd ../feature-auth
git submodule update --init --recursive
```

### Automatic Submodule Handling

When creating worktrees with submodules, automated initialization:

```bash
# Worker mode automatically handles this:
git worktree add ../feature-auth feature/auth
cd ../feature-auth
git submodule update --init --recursive
# ✓ Worktree created with submodules initialized
# ⚠ Remember: Submodule state is per-worktree
```

### Submodule Best Practices with Worktrees

1. **Always initialize submodules** after creating worktree
2. **Be aware** each worktree can have different submodule commits
3. **Update submodules** independently in each worktree
4. **Document** expected submodule state for each branch
5. **Use automation** to handle submodule initialization consistently

## Common Pitfalls & Solutions

### Pitfall 1: Forgetting to Remove Worktrees

**Problem**: Worktrees accumulate, waste disk space

**Solution - Manual Cleanup**:

```bash
# List all worktrees
git worktree list

# Remove specific worktree
git worktree remove ../stale-feature

# Prune deleted worktrees
git worktree prune
```

**Solution - Automated Cleanup**:

```bash
# Find stale worktrees (no recent commits)
git worktree list | while read path branch commit; do
  last_commit_date=$(git -C "$path" log -1 --format=%cd --date=short 2>/dev/null)
  echo "$path - Last commit: $last_commit_date"
done
```

### Pitfall 2: Trying to Checkout Same Branch

**Problem**: Cannot checkout same branch in multiple worktrees

```bash
# In main repo
git checkout feature/auth

# Try to create worktree
git worktree add ../other feature/auth
# Error: 'feature/auth' is already checked out at '/path/to/main-repo'
```

**Solution**: Use detached HEAD or different branch

```bash
# Option 1: Detached HEAD at same commit
git worktree add ../other-work $(git rev-parse feature/auth)

# Option 2: Create new branch
git worktree add ../new-work -b feature/auth-v2 feature/auth
```

### Pitfall 3: Deleting Worktree Directory Manually

**Problem**: Deleted worktree directory but git still tracks it

```bash
rm -rf ../old-worktree  # Manual deletion

git worktree list
# Still shows ../old-worktree (but path doesn't exist!)
```

**Solution**: Prune stale references

```bash
git worktree prune
```

**Better**: Always use `git worktree remove`

```bash
git worktree remove ../old-worktree
```

### Pitfall 4: Moving Repository with Worktrees

**Problem**: Moved repository - worktrees now point to wrong location

```bash
# Original structure
/home/user/projects/my-repo/
/home/user/projects/feature-a/

# After moving
/home/user/dev/my-repo/      # Moved here
/home/user/projects/feature-a/  # Still here - now broken!
```

**Solution**: Repair worktrees

```bash
cd /home/user/dev/my-repo
git worktree repair /home/user/projects/feature-a
```

Or move worktrees with repository:

```bash
mv /home/user/projects /home/user/dev/
cd /home/user/dev/my-repo
git worktree list  # Automatically updated
```

### Pitfall 5: Worktree with Uncommitted Changes

**Problem**: Removing worktree with uncommitted work

```bash
git worktree remove ../feature-auth
# Error: '../feature-auth' contains modified or untracked files
```

**Solution 1**: Force remove (loses changes!)

```bash
git worktree remove --force ../feature-auth
```

**Solution 2**: Commit or stash first (recommended)

```bash
cd ../feature-auth
git add .
git commit -m "WIP: Save before removing worktree"
# or
git stash push -m "Work from worktree"

cd ../main-repo
git worktree remove ../feature-auth
```

## Performance Considerations

### Disk Usage Comparison

```md
Repository size: 1 GB (.git)
Working directory: 500 MB

Traditional clones:
  repo-1: 1.5 GB (.git + working)
  repo-2: 1.5 GB
  repo-3: 1.5 GB
  Total: 4.5 GB

Worktrees:
  main-repo: 1.5 GB (.git + working)
  worktree-1: 500 MB (working dir only)
  worktree-2: 500 MB (working dir only)
  Total: 2.5 GB

Savings: 2 GB (44% reduction)
```

### Creation Speed

- **Fast**: Worktrees created instantly (no cloning)
- **Sparse checkout**: Even faster for large repos
- **Submodules**: Slower (must initialize separately)

### Git Operations

- Operations in one worktree don't affect others (independent working state)
- Fetch/push affects all worktrees (shared refs)
- Checkout is per-worktree
- Rebase/merge is per-worktree (but updates shared refs)

## Cleanup Strategies

### Cleanup Criteria

**Safe to Remove**:

- Branch has been merged
- No uncommitted changes
- No unmerged commits
- Inactive for > 30 days

**Requires Review**:

- Unmerged commits
- Uncommitted changes
- Active within 30 days
- Important feature work

### Manual Cleanup Commands

```bash
# List all worktrees
git worktree list

# Remove specific worktree (safe)
git worktree remove ../feature-done

# Remove with force (loses uncommitted changes)
git worktree remove --force ../old-work

# Prune stale references
git worktree prune
```

## Examples

### Example 1: Parallel Feature Development

```bash
# Working on two features simultaneously
git worktree add ../auth feature/authentication
git worktree add ../payments feature/payment-gateway

# Terminal 1: Authentication work
cd ../auth
code .
# Develop authentication, run auth tests

# Terminal 2: Payment work
cd ../payments
code .
# Develop payments, run payment tests

# No context switching, no stashing
# Each feature has independent state
```

### Example 2: Emergency Hotfix During Feature Work

```bash
# Current state: Working on feature with uncommitted changes
git status
# On branch feature/new-dashboard
# Changes not staged for commit:
#   modified: src/dashboard.js

# Emergency bug report!
# Create hotfix worktree without affecting current work
git worktree add ../hotfix -b hotfix/security-patch main

cd ../hotfix
# Fix bug, test, commit, push
vim src/auth.js
git add src/auth.js
git commit -m "Fix authentication bypass vulnerability"
git push origin hotfix/security-patch

# Return to feature work - nothing changed!
cd ../my-repo
git status
# Still shows uncommitted changes to dashboard.js
```

### Example 3: Code Review in Isolation

```bash
# Fetch PR for review
git fetch origin pull/456/head:review-ui-redesign

# Create isolated review worktree
git worktree add ../code-review review-ui-redesign

cd ../code-review
# Run tests
npm test

# Try experimental changes
vim src/components/Header.js
# Make changes to understand code better

# Leave worktree (changes don't matter)
cd ../my-repo

# Cleanup after review
git worktree remove --force ../code-review
git branch -d review-ui-redesign
```

## Best Practices

**Do**:

- ✅ Create worktrees for parallel branch work requiring separate context
- ✅ Use descriptive worktree directory names matching branch purpose
- ✅ Initialize submodules in each worktree after creation
- ✅ Remove worktrees when work completes to free disk space
- ✅ Use `git worktree remove` instead of manual directory deletion
- ✅ Leverage worktrees for hotfix workflows and code review
- ✅ Combine worktrees with sparse checkout for large repositories

**Don't**:

- ❌ Create worktrees for simple branch switches (use git checkout)
- ❌ Manually delete worktree directories (use git worktree remove)
- ❌ Checkout same branch in multiple worktrees
- ❌ Forget to clean up old worktrees (monitor disk usage)
- ❌ Move repository without repairing worktree references
- ❌ Use worktrees when single-task focus would be simpler

## Common Pitfalls

**Pitfall 1: Accumulating stale worktrees**

- Worktrees created for temporary tasks but never removed
- Solution: Regularly audit worktrees with `git worktree list`, remove completed work

**Pitfall 2: Submodule state confusion**

- Each worktree has independent submodule state
- Solution: Always run `git submodule update --init --recursive` after creating worktree

**Pitfall 3: Moving repository breaks worktrees**

- Worktree paths become invalid when repository location changes
- Solution: Use `git worktree repair` or move all directories together
