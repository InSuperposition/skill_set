---
name: skill-git-submodule
description: Master-level expertise in git submodules for embedding external repositories. Specialized in conservative update strategies, development vs consumption workflows, nested submodule management, and submodule-worktree integration.
allowed-tools: Read
---

# Git Submodule - External Repository Management

## Overview

Git submodules enable embedding one git repository inside another as a subdirectory while keeping their histories separate. This powerful feature is essential for managing external dependencies, shared code across projects, vendored libraries, and plugin architectures without merging external history into your repository.

Submodules provide explicit control over dependency versions by tracking specific commits, maintain separate git histories for parent and child repositories, and enable multiple projects to share the same code. However, they require careful management and understanding of their conservative update philosophy to avoid common pitfalls.

This skill advocates a conservative, safety-first approach to submodules: manual and deliberate updates with testing, pinning to specific commits rather than tracking branches, and awareness of when alternative approaches (subtrees, monorepos, package managers) might be more appropriate.

## Requirements

- Git v2.5.0+ (minimum version for worktree support)
- Understanding of git repositories and commit tracking
- SSH or HTTPS access to submodule repositories

## Core Capabilities

- **Repository Embedding** - Include external git repositories as subdirectories with separate histories
- **Version Pinning** - Track submodules at specific commits for stability and reproducibility
- **Conservative Updates** - Manual, deliberate update workflow with testing before committing
- **Development Workflow** - Work inside submodules with branch management and coordinated pushes
- **Consumption Workflow** - Use external libraries without modification, update deliberately
- **Nested Submodule Handling** - Manage (or flatten) multi-level submodule hierarchies
- **Worktree Integration** - Initialize submodules per-worktree with state awareness
- **Alternative Analysis** - Recommend when to use subtrees, monorepos, or package managers instead

## When to Use

**Use git-submodule for**:

- External libraries or tools you don't control but need specific versions
- Shared components used across multiple independent projects
- Vendored dependencies requiring version control and history
- Plugin architectures with separate repositories
- Dependencies with independent release cycles
- Need to track specific versions with explicit update control
- Want separate git histories maintained
- Multiple projects sharing same codebase

**Don't use git-submodule for**:

- Language-specific dependencies (use package managers: npm, pip, cargo)
- Frequent dependency updates (consider vendoring or package managers)
- Single team controlling all code (consider monorepo)
- Simple copy-paste needs (consider git subtree or vendoring)

## Integration with Other Skills

- **first-principles**: Question whether external dependency management is necessary, consider simpler vendoring
- **simplicity**: Prefer package managers or subtrees when submodule complexity isn't justified
- **git-worker**: Execute submodule operations with safety checks and validation
- **git-worktree**: Coordinate submodule initialization across multiple worktrees
- **git-safety**: Implement safe submodule update and removal workflows
- **git-advanced**: Use shallow clones and sparse checkout with submodules

## Core Concepts

### The Problem Submodules Solve

You need to include external code in your project:

- Third-party libraries
- Shared components across projects
- Vendored dependencies
- Plugin architectures

**Traditional Approach Problems**:

- Copy/paste: No update mechanism, no history
- Package managers: May not fit your workflow
- Git subtree: Merges external history into your repo
- Monorepo: Not always practical

**Submodule Solution**:

- External repository tracked at specific commit
- Separate history maintained
- Explicit control over updates
- Multiple projects can share same submodule

### How Submodules Work

```sh
my-project/
├── .git/
├── .gitmodules              # Submodule configuration
├── src/
│   └── main.c
└── external/
    └── lib-foo/            # Submodule directory
        ├── .git            # Points to .git/modules/external/lib-foo
        ├── src/
        └── README.md

.git/modules/external/lib-foo/  # Actual submodule repository
```

**Key Points**:

- `.gitmodules`: Maps submodule paths to URLs
- `.git/modules/<path>`: Actual submodule git repository
- Submodule directory: Working directory with `.git` file (not directory)
- Parent repo tracks: Submodule URL + specific commit hash

## Basic Operations

### Adding Submodules

**Add submodule at specific path**:

```bash
git submodule add https://github.com/user/repo.git path/to/submodule
```

**Add submodule with specific branch tracking**:

```bash
git submodule add -b develop https://github.com/user/repo.git path/to/submodule
```

**Add submodule with custom name**:

```bash
git submodule add --name custom-name https://github.com/user/repo.git path/to/submodule
```

### Cloning Projects with Submodules

**Option 1: Clone then initialize**:

```bash
git clone https://github.com/user/parent-repo.git
cd parent-repo
git submodule update --init --recursive
```

**Option 2: Clone with submodules** (recommended):

```bash
git clone --recurse-submodules https://github.com/user/parent-repo.git
```

### Updating Submodules

**Update to latest commit on tracked branch**:

```bash
git submodule update --remote
```

**Update specific submodule**:

```bash
git submodule update --remote path/to/submodule
```

**Update and merge** (instead of checkout):

```bash
git submodule update --remote --merge
```

### Removing Submodules

**Modern removal** (git >= 1.8.3):

```bash
git rm path/to/submodule
git commit -m "Remove submodule"
```

**Complete cleanup** (optional):

```bash
# Remove from .git/modules for complete cleanup
rm -rf .git/modules/path/to/submodule
```

## Conservative Update Strategy

**Philosophy**: Submodules should be updated **manually and deliberately**.

### Why Conservative?

1. **Stability**: Automatic updates can break your build
2. **Testing**: Each submodule update should be tested before committing
3. **Version Control**: Parent repo should explicitly track submodule versions
4. **Team Coordination**: Updates should be coordinated across team members

### Recommended Workflow

```bash
# 1. Check current submodule status
git submodule status

# 2. Update specific submodule to latest
cd path/to/submodule
git fetch
git log --oneline HEAD..origin/main  # Review changes
cd ../..

# 3. Test with new version
git submodule update --remote path/to/submodule
make test

# 4. If tests pass, commit the update
git add path/to/submodule
git commit -m "Update submodule to latest version

Changes:
- Feature X added
- Bug Y fixed
Tested: All tests passing"

# 5. Push to share with team
git push
```

### Configuration for Conservative Approach

```bash
# Prevent accidental recursive updates
git config submodule.recurse false

# Require explicit submodule operations
git config status.submoduleSummary true
git config diff.submodule log
```

## Development vs Consumption Workflows

### Consumption Workflow

**Use Case**: You use a library but don't modify it

**Best Practices**:

1. **Pin to specific commits** (not branches)
2. **Update deliberately** with testing
3. **Document** why each update was made
4. **Vendor** critical dependencies for stability

```bash
# Add submodule pinned to commit
git submodule add https://github.com/user/library.git vendor/library
cd vendor/library
git checkout v1.2.3  # Pin to release tag
cd ../..
git add vendor/library .gitmodules
git commit -m "Add library v1.2.3 as submodule"

# Update only after testing
cd vendor/library
git fetch
git checkout v1.3.0
cd ../..
make test  # Test before committing!
git add vendor/library
git commit -m "Update library to v1.3.0 (tested)"
```

### Development Workflow

**Use Case**: You actively develop the submodule

**Best Practices**:

1. **Work inside submodule** directory
2. **Create branches** for development
3. **Push submodule** first, then parent
4. **Coordinate** submodule and parent commits

```bash
# Setup for development
cd path/to/submodule
git checkout -b feature/new-functionality
# Make changes
git add .
git commit -m "Add new functionality"
git push origin feature/new-functionality

# Update parent repo
cd ../..
git add path/to/submodule
git commit -m "Update submodule with new functionality"
git push
```

**Challenges**:

- Two repositories to manage
- Push order matters (submodule first!)
- Branch management across repos
- CI/CD complexity

**When to Reconsider**:

- If you're the only user → Consider merging into parent repo
- If changes are frequent → Consider monorepo approach
- If coordination is difficult → Consider git subtree

## Nested Submodules

**Philosophy**: **Avoid deep nesting** - it increases complexity exponentially.

### Why Nesting is Problematic

1. **Complexity**: N levels = N update operations
2. **Performance**: Slower clone, update, status operations
3. **Confusion**: Tracking what depends on what
4. **Debugging**: Errors can propagate through levels

### Maximum Recommended Depth

- **Ideal**: 0 levels (no nesting)
- **Acceptable**: 1 level (submodules of submodules)
- **Warning**: 2+ levels (expert territory)

### Handling Existing Nested Submodules

```bash
# Initialize all levels
git submodule update --init --recursive

# Check nesting depth
git submodule foreach --recursive 'echo $name at depth $sm_path'

# Update all levels
git submodule update --remote --recursive
```

### Flattening Nested Submodules

```bash
# Instead of: parent -> lib-a -> lib-b
# Do: parent -> lib-a
#     parent -> lib-b

# Add nested submodule at top level
cd vendor/lib-a/vendor/lib-b
git remote get-url origin  # Get URL

cd ../../../../  # Back to parent
git submodule add <url> vendor/lib-b

# Remove nested reference from lib-a
cd vendor/lib-a
git rm vendor/lib-b
git commit -m "Remove nested submodule (now at top level)"

cd ../..
git add vendor/lib-a vendor/lib-b
git commit -m "Flatten submodule structure"
```

## Submodules vs Alternatives

### When to Use Submodules

**Ideal Use Cases**:

- External libraries you don't control
- Multiple projects sharing same code
- Need to track specific versions
- Want separate git histories
- Dependencies have independent release cycles

### When to Use Git Subtree

**Better for**:

- Want simpler workflow (no special clone/update commands)
- Don't need to push changes back to dependency
- Want to merge external history into your repo
- Team struggles with submodule complexity

```bash
# Add subtree (alternative to submodule)
git subtree add --prefix=vendor/library https://github.com/user/library.git main --squash

# Update subtree
git subtree pull --prefix=vendor/library https://github.com/user/library.git main --squash
```

**Trade-off**: Merges external history into your repo (larger repo)

### When to Use Monorepo

**Better for**:

- Single team controlling all code
- Need atomic commits across projects
- Want simplified build/test
- Have good monorepo tooling (Bazel, Nx, etc.)

**Trade-off**: Larger repo, all code together

### When to Use Package Managers

**Better for**:

- Language-specific dependencies
- Need version resolution
- Want semantic versioning
- Published packages (npm, pip, cargo, etc.)

**Trade-off**: Language-specific, requires package registry

### Decision Matrix

| Need | Submodules | Subtree | Monorepo | Package Manager |
| ---- | ---------- | ------- | -------- | --------------- |
| Separate histories | ✓ | ✗ | ✗ | ✓ |
| Simple workflow | ✗ | ✓ | ✓ | ✓ |
| Version pinning | ✓ | Manual | N/A | ✓ |
| Two-way development | ✓ | ✓ | ✓ | ✗ |
| Cross-project | ✓ | ✗ | ✓ | ✓ |

## Submodules + Worktrees

### The Challenge

Submodules are **per-worktree**, not shared:

```sh
main-repo/
├── .git/
├── src/
└── vendor/lib-a/      # Submodule at commit abc123

../feature-worktree/
├── .git -> ../main-repo/.git
├── src/
└── vendor/lib-a/      # Different commit def456 possible!
```

### Solution: Initialize Per-Worktree

```bash
# Create worktree
git worktree add ../feature-work feature/new-feature

# Initialize submodules in worktree
cd ../feature-work
git submodule update --init --recursive
```

### Automatic Initialization

Worker mode handles this automatically:

```bash
# Create worktree with submodules
git worktree add ../feature-auth feature/auth
cd ../feature-auth
git submodule update --init --recursive
# ✓ Worktree created with submodules initialized
# ⚠ Remember: Submodule state is per-worktree
```

## Common Pitfalls & Solutions

### Pitfall 1: Detached HEAD in Submodule

**Problem**: Submodule is in detached HEAD state

```bash
cd vendor/library
git status
# HEAD detached at abc1234
```

**Why**: Submodules check out specific commits, not branches

**Solution 1**: This is normal for consumption workflow (no action needed)

**Solution 2**: For development, checkout a branch

```bash
cd vendor/library
git checkout main
# Make changes
git commit -m "Update"
git push
cd ../..
git add vendor/library
git commit -m "Update submodule"
```

### Pitfall 2: Forgetting to Push Submodule

**Problem**: Pushed parent repo, but not submodule commits

```bash
# Team members get error: reference is not a tree
```

**Solution**: Always push submodule first

```bash
cd vendor/library
git push
cd ../..
git push
```

**Better**: Use push safety check

```bash
git config push.recurseSubmodules check  # Warn before push
# or
git config push.recurseSubmodules on-demand  # Auto-push submodules
```

### Pitfall 3: Submodule URL Changes

**Problem**: Submodule URL changed (renamed repo, moved org)

**Solution**: Update .gitmodules and sync

```bash
# Edit .gitmodules
vim .gitmodules
# Change url = old-url to url = new-url

# Sync configuration
git submodule sync
git submodule update --init
```

### Pitfall 4: Local Changes in Submodule

**Problem**: Accidentally modified submodule without committing

```bash
git status
# modified: vendor/library (modified content)
```

**Solution 1**: Discard changes

```bash
git submodule update --force vendor/library
```

**Solution 2**: Commit changes

```bash
cd vendor/library
git add .
git commit -m "Local modifications"
cd ../..
git add vendor/library
git commit -m "Update submodule with local changes"
```

### Pitfall 5: Submodule Not Initialized

**Problem**: Cloned repo, submodule directories empty

**Solution**: Initialize submodules

```bash
git submodule update --init --recursive
```

**Better**: Clone with submodules

```bash
git clone --recurse-submodules <url>
```

## Advanced Patterns

### Pattern 1: Submodule with Specific Commit

```bash
# Add submodule
git submodule add https://github.com/user/lib.git vendor/lib

# Pin to specific commit
cd vendor/lib
git checkout abc1234
cd ../..
git add vendor/lib
git commit -m "Pin lib to specific commit abc1234"
```

### Pattern 2: Submodule Branch Tracking

```bash
# Add submodule tracking a branch
git submodule add -b stable https://github.com/user/lib.git vendor/lib

# Update to latest on branch
git submodule update --remote --merge vendor/lib
```

### Pattern 3: Submodule for Private Repos

```bash
# Use SSH URL for private repo
git submodule add git@github.com:user/private-lib.git vendor/private

# Team members need SSH access
```

### Pattern 4: Submodule with Shallow Clone

```bash
# Add submodule with shallow history
git submodule add --depth 1 https://github.com/user/large-lib.git vendor/large
```

## Examples

### Example 1: Adding External Library as Submodule

```bash
# Add library at specific version
git submodule add https://github.com/nlohmann/json.git vendor/json
cd vendor/json
git checkout v3.11.2  # Pin to stable release
cd ../..

# Commit submodule addition
git add .gitmodules vendor/json
git commit -m "Add nlohmann/json v3.11.2 as submodule"

# Team members clone
git clone --recurse-submodules <your-repo-url>
```

### Example 2: Updating Submodule After Testing

```bash
# Check current version
cd vendor/json
git describe --tags
# v3.11.2

# Review available updates
git fetch
git log --oneline HEAD..origin/develop

# Update to new version
git checkout v3.11.3
cd ../..

# Test with new version
make test

# If tests pass, commit update
git add vendor/json
git commit -m "Update json library to v3.11.3

Changes:
- Performance improvements
- Bug fixes for edge cases
Tested: All tests passing"
```

### Example 3: Developing Inside Submodule

```bash
# Create feature branch in submodule
cd vendor/shared-component
git checkout -b feature/new-api

# Make changes
vim src/api.c
git add src/api.c
git commit -m "Add new API endpoint"

# Push submodule first
git push origin feature/new-api

# Update parent repo
cd ../..
git add vendor/shared-component
git commit -m "Update shared-component with new API"
git push
```

## Best Practices

**Do**:

- ✅ Pin submodules to specific commits or tags for stability
- ✅ Test updates before committing submodule pointer changes
- ✅ Push submodule commits before pushing parent repository
- ✅ Use `git clone --recurse-submodules` for initial clone
- ✅ Document submodule update procedures for your team
- ✅ Configure `push.recurseSubmodules check` for safety
- ✅ Review submodule changes with `git diff --submodule=log`

**Don't**:

- ❌ Track branches directly (pin to specific commits/tags)
- ❌ Update submodules automatically without testing
- ❌ Nest submodules deeply (prefer flat structure)
- ❌ Use submodules for language-specific packages (use package managers)
- ❌ Forget to initialize submodules in new worktrees
- ❌ Make local changes in consumption workflow (commit to submodule repo first)
- ❌ Push parent before pushing submodule commits

## Common Pitfalls

**Pitfall 1: Submodule in detached HEAD causing confusion**

- Detached HEAD is normal for submodules (tracking specific commits)
- For consumption: No action needed
- For development: Checkout a branch before making changes

**Pitfall 2: Pushing parent without pushing submodule**

- Parent repo references commit that doesn't exist remotely
- Team members cannot checkout
- Solution: Configure `push.recurseSubmodules check` or always push submodule first

**Pitfall 3: Deep nesting complexity**

- Each nesting level multiplies maintenance overhead
- Solution: Flatten structure by bringing nested submodules to top level
