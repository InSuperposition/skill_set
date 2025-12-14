---
name: skill-git-advanced
description: On-demand advanced Git features: sparse checkout, partial clone, Git LFS, hooks automation, bisect, filter-repo, attributes, and offline transfer tools.
allowed-tools: Read
---

# Git Advanced - Powerful Tools for Specific Scenarios

## Overview

Advanced Git features can drastically improve workflows for large repos, binary assets, automation, and debugging—but they add complexity and should be used intentionally.

This skill provides practical guidance for when to use (and when to avoid) sparse checkout, partial clones, Git LFS, hooks, bisect, and history rewriting tools like `git-filter-repo`.

## Requirements

- Git v2.5.0+
- Optional tools depending on feature:
  - `git-lfs`
  - `git-filter-repo`

## Core Capabilities

- Sparse checkout (cone vs non-cone) and sparse worktrees
- Partial clone filters and shallow clones for CI/exploration
- Git LFS workflows and migrations
- Hooks and hook frameworks (pre-commit, Husky)
- `git bisect` manual and automated regression hunting
- `git filter-repo` safety workflows (backup-first)
- `.gitattributes` for diffs, line endings, merge drivers
- Offline transfer via `git archive` and `git bundle`

## When to Use

**Use advanced for**:

- Very large repositories (slow clone/status/build)
- Large binary assets or datasets that don’t belong in normal history
- Enforcing checks before commit/push
- Finding a regression across many commits
- Removing sensitive data or shrinking history (with full coordination)

**Don’t use advanced for**:

- Small repos where standard Git is already fast and clear
- Teams that won’t adopt the same workflow (risk of confusion)
- History rewriting on shared repos without explicit coordination

## Integration with Other Skills

- `git-worktree`: sparse checkout per worktree; bisect worktrees
- `git-health`: detect when advanced tools are warranted
- `git-safety`: backup/confirm before `filter-repo`, LFS migrations, force pushes
- `git-config`: LFS and large-repo feature flags

## Key Concepts

### Sparse Checkout

```bash
git clone --no-checkout --filter=blob:none <url>
cd repo
git sparse-checkout init --cone
git sparse-checkout set src/auth tests/auth
git checkout main
```

Non-cone mode (patterns):

```bash
git sparse-checkout init --no-cone
echo 'src/*/tests/' >> .git/info/sparse-checkout
echo '!src/legacy/' >> .git/info/sparse-checkout
git sparse-checkout reapply
```

### Partial Clone / Shallow Clone

```bash
git clone --filter=blob:none <url>
git clone --filter=tree:0 <url>
git clone --depth 1 --single-branch --branch=main <url>
```

### Git LFS

```bash
git lfs install
git lfs track "*.psd"
git add .gitattributes
git add large-file.psd
git commit -m "Track design assets with LFS"
git push
```

Migration (history rewrite; backup first):

```bash
git lfs migrate import --include="*.psd" --everything
```

### Hooks & Automation

Hooks live in `.git/hooks/`. Example `pre-commit`:

```bash
#!/bin/bash
npm run lint || exit 1
npm test || exit 1
```

### Bisect

```bash
git bisect start
git bisect bad
git bisect good v1.0.0
git bisect run ./test.sh
git bisect reset
```

Bisect in a separate worktree:

```bash
git worktree add ../bisect-session main
cd ../bisect-session
```

### Filter-Repo (History Rewriting)

Install:

```bash
brew install git-filter-repo
pip install git-filter-repo
```

Remove a file from all history:

```bash
git filter-repo --path secrets.txt --invert-paths
```

Safety workflow:

```bash
git clone --mirror . ../backup.git
git clone ../backup.git working-copy
cd working-copy
git filter-repo <options>
```

### .gitattributes

```gitattributes
* text=auto
*.sh text eol=lf
*.bat text eol=crlf
*.psd filter=lfs diff=lfs merge=lfs -text
```

### Archive and Bundle

```bash
git archive --format=tar.gz --output=project.tar.gz main
git bundle create repo.bundle --all
git clone repo.bundle repo-from-bundle
```

## Examples

### Example: Faster clone for CI

```bash
git clone --depth 1 --single-branch --branch=main --filter=blob:none <url>
```

### Example: Shrink repo by removing large blobs (dangerous)

```bash
git clone --mirror . ../backup.git
git clone ../backup.git working-copy
cd working-copy
git filter-repo --strip-blobs-bigger-than 10M
```

## Best Practices

**Do**:

- Start simple; add advanced features only when a pain is proven
- Use backups and coordination for all history rewrites
- Keep hooks fast; move heavy checks to CI

**Don’t**:

- Rewrite shared history casually
- Adopt sparse/partial workflows if build tooling can’t handle them
