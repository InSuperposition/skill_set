---
name: skill-git-recovery
description: Crisis management for Git mistakes using reflog, fsck, and safety-first recovery workflows. Recover lost commits, branches, and undo destructive operations.
allowed-tools: Read
---

# Git Recovery - Reflog, Undo, and Disaster Playbooks

## Overview

Git’s reflog is the safety net: it records where `HEAD` and refs have pointed. Most “lost commits” are recoverable if you act before reflog entries expire (commonly 90 days).

This skill provides practical recovery playbooks for common disasters (bad reset, deleted branch, botched rebase, lost amend, dropped stash), plus prevention strategies that make recovery easy.

## Requirements

- Git v2.5.0+
- Access to the repository’s `.git/` directory (reflog must exist)

## Core Capabilities

- **Understand reflog** entries and what they mean
- **Recover lost commits** after `reset --hard`, rebase, or checkout mistakes
- **Restore deleted branches** from reflog history
- **Recover pre-amend** commit state
- **Recover stashes** (when possible) and understand limitations
- **Use `git fsck --lost-found`** to locate unreachable objects
- **Extend reflog retention** for safer long-lived repos

## When to Use

**Use recovery for**:

- “I lost commits after reset/rebase/amend”
- “I deleted a branch and need it back”
- “I force pushed and need to revert to old state”
- “I need to understand what happened and recover safely”

**Don’t use recovery for**:

- Restoring **untracked files** deleted by `git clean -fdx` (Git can’t recover these)
- Treating reflog as a substitute for backups on critical repos

## Integration with Other Skills

- `git-safety`: prevention checklists; create backups before risky ops
- `git-worker`: guided execution with safe stops and recovery options
- `git-rebase`: undo a bad interactive rebase
- `first-principles`: identify what exactly you lost and what success means (single commit vs whole branch)
- `simplicity`: prefer the simplest recovery that restores state without extra rewriting

## Key Concepts

### What reflog is

The reflog records movements of `HEAD` and refs due to:

- commits, checkouts, resets, merges, rebases, cherry-picks, etc.

View reflog:

```bash
git reflog
git reflog show --all
git reflog --no-abbrev
git reflog --relative-date
```

Search reflog:

```bash
git reflog --grep="rebase"
git reflog | grep "checkout: moving"
```

### The safest recovery pattern

1. Inspect reflog.
2. Create a recovery branch at the target state.
3. Verify.
4. Merge/cherry-pick back if needed.

```bash
git reflog
git branch recovery HEAD@{N}
git log recovery --oneline -5
```

## Common Recovery Scenarios

### 1) Undo `git reset --hard`

```bash
git reflog
git reset --hard HEAD@{1}
```

Prevention:

```bash
git branch backup-before-reset
```

### 2) Recover a deleted branch

```bash
git reflog show --all | grep "feature/auth"
git branch feature/auth <sha>
```

Or with reflog index:

```bash
git branch feature/auth HEAD@{5}
```

### 3) Recover from a bad rebase

```bash
git reflog
git reset --hard HEAD@{before-rebase}
```

During rebase, if you notice problems early:

```bash
git rebase --abort
```

### 4) Recover a commit after `--amend`

```bash
git reflog
git show HEAD@{1}
git branch recover-original HEAD@{1}
```

Or cherry-pick it:

```bash
git cherry-pick HEAD@{1}
```

### 5) Recover dropped stash (sometimes)

```bash
git reflog show stash
```

If needed, search unreachable commits:

```bash
git fsck --unreachable | grep commit | cut -d ' ' -f3 | xargs git log --oneline --no-walk
```

Then apply:

```bash
git stash apply <hash>
```

### 6) After `git clean -fdx` (limitations)

Untracked files are not in Git history and cannot be recovered via reflog.

Always dry-run:

```bash
git clean -fdx --dry-run
git clean -fdn
```

### 7) Rebased a published branch (team impact)

If you haven’t pushed yet:

```bash
git reflog
git reset --hard HEAD@{before-rebase}
```

If you already pushed: coordinate. Often better to **revert** than force push again.

## Advanced Tools

### Cross-branch recovery and unreachable objects

```bash
git reflog --all
git fsck --lost-found
```

### Retention policies

```bash
git config gc.reflogExpire 90
git config gc.reflogExpireUnreachable 30

git config --global gc.reflogExpire 180
git config --global gc.reflogExpireUnreachable 60
```

Avoid aggressive expiration: it destroys your safety net.

## Prevention Strategies

- Create backup branches/tags before risky work:

```bash
git branch backup/$(date +%Y%m%d-%H%M%S)
git tag backup-$(date +%Y%m%d-%H%M%S)
```

- Prefer `git push --force-with-lease`, not `--force`.
- Keep operations small and verifiable.

## Examples

### Example: Recover and inspect before restoring

```bash
git reflog
git branch recovery HEAD@{3}
git log recovery --oneline -10
git checkout recovery
```

## Best Practices

**Do**:

- Stop and inspect `git reflog` immediately after a mistake
- Create a recovery branch before making more changes
- Verify with `git log`, `git diff`, and tests

**Don’t**:

- Run additional destructive commands “to fix it” before checking reflog
- Expire reflog aggressively
- Assume untracked files are recoverable
