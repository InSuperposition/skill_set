---
name: skill-git-rebase
description: Master-level guidance for safe rebasing and interactive history cleanup. Focuses on the golden rule, conflict workflows, autosquash, and recovery strategies.
allowed-tools: Read
---

# Git Rebase - Clean History, Safely

## Overview

Rebasing rewrites commit history by replaying commits on a new base. Used well, it produces a clean, linear history that is easier to review, bisect, and understand. Used poorly, it can disrupt collaborators and lose work.

This skill focuses on **safety-first rebasing**: choosing rebase vs merge vs squash appropriately, performing interactive rebase cleanups, handling conflicts, and recovering when things go wrong.

## Requirements

- Git v2.5.0+ (baseline compatibility)
- Comfort with branches, commits, and remotes

## Core Capabilities

- **Rebase vs merge vs squash** selection with tradeoffs
- **Interactive rebase**: reorder, squash, fixup, drop, reword, edit
- **Autosquash workflow** (`--fixup`, `rebase.autoSquash`)
- **Safety protocol**: backups, pre-flight checks, abort/continue patterns
- **Conflict handling** and progress inspection during rebase
- **Advanced rebases**: `--onto`, `--exec`, `--rebase-merges`
- **Recovery** using reflog and backup branches

## When to Use

**Use rebase for**:

- Updating an unpublished feature branch with latest `main`
- Cleaning up local/WIP commits before opening a PR
- Consolidating fixups into the right commits (autosquash)
- Creating a readable, review-friendly commit series

**Don’t use rebase for**:

- Rewriting commits that other people have already based work on
- “Fixing” published history on shared branches without coordination
- Situations where preserving branch structure and merge context matters

## Integration with Other Skills

- `first-principles`: validate whether clean history is required vs “good enough”
- `simplicity`: prefer the simplest workflow that minimizes team disruption
- `git-worker`: execute rebases with backups and confirmations
- `git-recovery`: undo a bad rebase using reflog
- `git-safety`: apply pre-flight checks before any history rewrite

## Key Concepts

### Golden Rule

**Never rebase commits that have been pushed to shared branches.**

If a commit is “published” (others might have pulled it), rebasing rewrites it into a new commit ID and creates divergence for everyone.

### Rebase vs Merge vs Squash

#### Rebase

Replay commits on top of another branch:

```bash
git checkout feature/auth
git rebase main
```

Pros:

- Linear history
- Easier `git bisect`
- Cleaner PR review

Cons:

- Rewrites history (danger if published)
- Conflicts can be repeated across commits

#### Merge

Preserve history with a merge commit:

```bash
git checkout main
git merge feature/auth
```

Pros:

- Safe for published/shared branches
- Preserves branch context

Cons:

- More complex `git log` on busy repos

#### Squash merge

Collapse a feature into one commit on the target branch:

```bash
git checkout main
git merge --squash feature/auth
git commit -m "Add authentication feature"
```

Pros:

- Very clean mainline history
- Easy to revert whole feature

Cons:

- Loses per-commit history

### Interactive Rebase

Rebase and edit history interactively:

```bash
git rebase -i HEAD~5
git rebase -i main
```

Common actions in the todo list:

- `pick`: keep commit
- `reword`: edit message
- `edit`: stop and amend
- `squash`: combine, edit message
- `fixup`: combine, discard message
- `drop`: remove commit

#### Edit workflow

```bash
git rebase -i main
# mark a commit as "edit"

git add -A
git commit --amend
git rebase --continue
```

### Autosquash

Enable autosquash:

```bash
git config --global rebase.autoSquash true
```

Create fixup commits:

```bash
git commit --fixup=<sha>
git rebase -i --autosquash main
```

### Safety-First Rebase Checklist

1. Create a backup branch:

```bash
git branch backup/$(git branch --show-current)-before-rebase
```

2. Ensure clean working directory:

```bash
git status
git config rebase.autoStash true
```

3. Confirm you’re not rebasing published commits:

```bash
git branch -r --contains HEAD
```

4. Fetch latest:

```bash
git fetch origin
```

### Conflict Handling During Rebase

```bash
git status
# resolve conflicts
git add <files>
git rebase --continue

# alternatives:
git rebase --skip
git rebase --abort
```

Inspect progress:

```bash
git log -1 REBASE_HEAD
cat .git/rebase-merge/git-rebase-todo 2>/dev/null || true
```

### Advanced Techniques (Use With Care)

Rebase onto a different base:

```bash
git rebase --onto develop main feature/auth
```

Run a command after each commit:

```bash
git rebase -i --exec "make test" main
```

Preserve merges:

```bash
git rebase -i --rebase-merges main
```

## Examples

### Example 1: Update a feature branch before PR

```bash
git checkout feature/auth
git fetch origin
git branch backup/feature-auth-before-rebase
git rebase origin/main
```

### Example 2: Clean up commit history with autosquash

```bash
git checkout feature/auth
git commit --fixup=HEAD~3
git rebase -i --autosquash main
```

### Example 3: Undo a bad rebase

```bash
git reflog
git reset --hard HEAD@{before-rebase}
```

## Best Practices

**Do**:

- Make a backup branch before any interactive rebase
- Prefer `--force-with-lease` if you must force push
- Rebase in smaller chunks if conflicts are overwhelming
- Run tests after rewriting history

**Don’t**:

- Rebase shared branch history without coordination
- Force push to protected branches by habit
- Use `--rebase-merges` unless you truly need merge structure

## Common Pitfalls

- **Rebasing published commits**: causes divergence and broken collaboration
- **Losing work during interactive rebase**: recover via backup branch or `git reflog`
- **Duplicate commits after rebase**: likely rebased already-merged commits; use merge or drop duplicates during `-i`
