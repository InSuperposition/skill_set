---
name: skill-git-safety
description: Safety-first protocols for git operations: pre-flight checks, backups, protected branches, confirmations, and recovery instructions to prevent data loss and team disruption.
allowed-tools: Read
---

# Git Safety - Pre-Flight, Backups, Protected Branches

## Overview

Git is powerful enough to lose work quickly. This skill provides a safety system for all git operations: classify risk, run pre-flight checks, create backups before destructive actions, and apply confirmation discipline—especially around published history and protected branches.

Use this skill as the default lens before any operation that can rewrite history, delete data, or disrupt collaborators.

## Requirements

- Git v2.5.0+

## Core Capabilities

- **Risk classification** (read-only → critical team impact)
- **Pre-flight checks**: clean working tree, branch status, remote status
- **Automatic backup patterns**: branch, stash, tag backups with verification
- **Protected branch enforcement** (main/master/develop/release/*)
- **Safe force push discipline** (`--force-with-lease`)
- **Recovery instructions** attached to risky operations

## When to Use

**Use safety for**:

- `reset --hard`, `clean -fdx`, history rewrites, force pushes
- Deleting branches/worktrees/submodules
- Large refactors and repo maintenance (`gc`, `prune`, history rewriting)

**Don’t use safety for**:

- Purely read-only inspection (`status`, `log`, `diff`) unless you’re preparing a risky op

## Integration with Other Skills

- `git-worker`: executes operations using these protocols and confirmations
- `git-recovery`: restores state when prevention fails
- `first-principles`: decide if the risky operation is truly necessary
- `simplicity`: prefer safer, simpler alternatives (merge/revert) over rewriting shared history

## Safety Levels (Mental Model)

- **Level 1**: Read-only (`status`, `log`, `diff`)
- **Level 2**: Low-risk local changes (`switch`, `stash`, `fetch`, `worktree add`)
- **Level 3**: Medium-risk local history/structure (`merge`, rebase unpushed, `branch -d`, `clean -fd`)
- **Level 4**: High-risk published history / destructive (`push --force*`, `reset --hard`, `clean -fdx`)
- **Level 5**: Critical team impact (protected branches, aggressive gc/prune, reflog expiration)

## Pre-Flight Checks

### Working directory

```bash
git status --porcelain
```

Detect:

- uncommitted changes
- conflicts / rebase in progress
- untracked files that matter (.env, credentials)

### Current branch and upstream

```bash
git rev-parse --abbrev-ref HEAD
git rev-parse --abbrev-ref --symbolic-full-name @{u}
git rev-list --left-right --count HEAD...@{u}
```

### Remote connectivity

```bash
git ls-remote --heads origin
git fetch --dry-run
git branch -vv
```

## Automatic Backups (Before Risk)

### Branch backup

```bash
BACKUP="backup/$(git branch --show-current)-$(date +%Y-%m-%d-%H%M%S)"
git branch "$BACKUP"
git rev-parse --verify "$BACKUP"
```

### Stash backup

```bash
git stash push -m "Auto-backup before risky op at $(date +%Y-%m-%d-%H%M%S)"
git stash list | head -1
```

### Tag backup

```bash
TAG="backup-$(date +%Y-%m-%d-%H%M%S)"
git tag "$TAG"
git tag | grep "$TAG"
```

## Protected Branches

Treat these as protected by default:

- `main`, `master`, `develop`, `production`, `staging`
- patterns like `release/*`, `hotfix/*`

On protected branches:

- avoid direct commits
- avoid force pushes
- prefer PRs and revert commits for rollback

## Confirmation Strategies

### Low/medium risk

Single confirmation: “Proceed? [y/N]”

### High risk

Show impact and require explicit typing:

- commits overwritten
- who might be affected
- backup created name

### Critical risk

Multiple explicit confirmations, including branch name and “I UNDERSTAND”.

## Safe Alternatives

### Prefer `--force-with-lease`

```bash
git push --force-with-lease origin feature/auth
```

### Prefer revert to rewriting shared history

```bash
git revert <bad-commit>
git push origin main
```

### Always dry-run `clean`

```bash
git clean -fd --dry-run
git clean -fdx --dry-run
```

## Examples

### Example: Safe branch deletion

```bash
git branch --merged main | grep -v "^\* main"
git log main..feature/auth --oneline
git branch -d feature/auth
```

### Example: Safe force push with backup

```bash
git branch backup/feature-auth-before-force
git push --force-with-lease origin feature/auth
```

## Best Practices

**Do**:

- Create a backup branch before rewriting history
- Fetch before analyzing ahead/behind
- Use `--force-with-lease`, not `--force`
- Keep a clear recovery plan for every risky operation

**Don’t**:

- Run `reset --hard` without a known good ref to return to
- Run `clean -fdx` without a dry-run and explicit confirmation
- Expire reflog aggressively (`git reflog expire`) without understanding the impact
