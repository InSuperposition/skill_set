---
name: skill-git-health
description: Repository health monitoring: detect bloat, branch hygiene issues, sync problems, submodule/worktree inconsistencies, config gaps, and corruption. Provides actionable fixes and safe automation ideas.
allowed-tools: Read
---

# Git Health - Keep Repos Fast, Clean, and Trustworthy

## Overview

Repository health checks keep git repositories efficient, understandable, and resilient. This skill focuses on monitoring and maintenance that prevents slow operations, confusing branch states, submodule/worktree surprises, and corruption.

Health checks should be actionable: surface critical issues early, provide a clear remediation path, and avoid noise.

## Requirements

- Git v2.5.0+

## Core Capabilities

- **Bloat detection**: large blobs, oversized `.git/`, binary history problems
- **Branch hygiene**: merged/stale branches, remote pruning opportunities
- **Commit quality signals**: WIP patterns, oversized commits
- **Remote sync status**: diverged branches, unpushed work
- **Submodule health**: detached heads, dirty states, unreachable remotes
- **Worktree health**: stale worktrees, prunable references
- **Config health**: missing safety settings and identity
- **Integrity checks**: `fsck`, pack verification signals

## When to Use

**Use health for**:

- Regular maintenance on active repositories
- Before/after big operations (rebase, cleanup, migration)
- Diagnosing “git is slow” or “repo feels messy”

**Don’t use health for**:

- One-off small repos where overhead isn’t worth it

## Integration with Other Skills

- `git-config`: apply recommended settings discovered by health checks
- `git-safety`: run before performing cleanup or deletion operations
- `git-worker`: optionally automate safe fixes (prune, delete merged branches)
- `git-advanced`: when bloat suggests LFS, partial clone, sparse checkout, filter-repo

## Key Concepts

### 1) Repository Bloat

Largest blobs:

```bash
git rev-list --objects --all | \
  git cat-file --batch-check='%(objecttype) %(objectname) %(objectsize) %(rest)' | \
  awk '/^blob/ {print substr($0,6)}' | \
  sort -nk2 | \
  tail -20
```

Size:

```bash
du -sh .git
```

### 2) Branch Hygiene

Merged branches:

```bash
git branch --merged main | grep -v "^\* main"
```

Remote prune dry-run:

```bash
git remote prune origin --dry-run
```

### 3) Commit Quality Signals

Count poor messages:

```bash
git log --pretty=format:"%s" --since="3 months ago" | grep -E "^(wip|WIP|fix|tmp)" | wc -l
```

Large commits (file count heuristic):

```bash
git log --all --pretty=format:"%H" --since="3 months ago" | \
  while read commit; do
    files=$(git diff-tree --no-commit-id --numstat -r $commit | wc -l)
    if [ $files -gt 50 ]; then echo "$commit: $files files"; fi
  done
```

### 4) Remote Sync Status

Fetch and compute ahead/behind:

```bash
git fetch --all --quiet
git for-each-ref --format="%(refname:short) %(upstream:short)" refs/heads/ | \
  while read local remote; do
    if [ -n "$remote" ]; then
      ahead=$(git rev-list --count $remote..$local)
      behind=$(git rev-list --count $local..$remote)
      if [ $ahead -ne 0 ] || [ $behind -ne 0 ]; then
        echo "$local: ahead $ahead, behind $behind"
      fi
    fi
  done
```

Unpushed local branches:

```bash
git branch -vv | grep -v "\\[origin"
```

### 5) Submodule Health

```bash
git submodule status
git submodule foreach 'git symbolic-ref -q HEAD || echo "Detached: $name"'
git submodule foreach --quiet 'test -n "$(git status --porcelain)" && echo "$name dirty"'
git submodule foreach 'git ls-remote --exit-code origin >/dev/null 2>&1 || echo "$name: remote unreachable"'
```

### 6) Worktree Health

```bash
git worktree list
git worktree prune --dry-run
```

### 7) Configuration Health

```bash
git config user.name
git config user.email
git config push.default
git config pull.rebase
git config fetch.prune
```

### 8) Integrity (Corruption Detection)

```bash
git fsck --quick
```

## Examples

### Example: Quick health sweep

```bash
du -sh .git
git remote prune origin --dry-run
git worktree prune --dry-run
git fsck --quick
```

## Best Practices

**Do**:

- Prune stale remote refs periodically (`git remote prune origin`)
- Delete merged branches to keep the graph understandable
- Use LFS or history rewrite tools when bloat is severe (with backups)
- Run `git fsck` if you suspect corruption or odd object errors

**Don’t**:

- Run destructive cleanup without `git-safety` protocols
- Hide bloat problems by “just cloning again” without fixing root cause
