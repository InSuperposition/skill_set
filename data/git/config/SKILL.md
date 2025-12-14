---
name: skill-git-config
description: Git configuration and optimization: safety-first defaults, quality-of-life aliases, platform-specific settings, and performance tuning for large repositories.
allowed-tools: Read
---

# Git Config - Safety, Ergonomics, Performance

## Overview

Git configuration has leverage: a few settings prevent common accidents, reduce merge pain, improve readability, and speed up large repositories. This skill recommends configs using a strict priority hierarchy:

1. Safety
2. Quality of life
3. Performance
4. Optional features (signing, credentials, LFS)

## Requirements

- Git v2.5.0+

## Core Capabilities

- Explain config scope and precedence (system/global/local)
- Recommend safety defaults for push/pull/fetch/rebase/merge
- Provide alias sets that improve daily workflows
- Suggest platform-specific settings (macOS/Windows/Linux)
- Offer performance tuning for large repos
- Review an existing config and identify gaps

## When to Use

**Use config for**:

- Setting up a new machine or repo
- Diagnosing “git is slow” or “git keeps surprising me”
- Standardizing team workflows (with explicit agreement)

**Don’t use config for**:

- Enforcing policy without team buy-in (use server-side protections)

## Integration with Other Skills

- `git-safety`: configs that reduce risk (push/pull defaults, rerere)
- `git-health`: config gaps detected as health issues
- `git-submodule`: push submodule safety (`push.recurseSubmodules`)
- `git-advanced`: LFS config and large-repo feature flags

## Key Concepts

### Config levels and precedence

```bash
git config --system <k> <v>   # /etc/gitconfig
git config --global <k> <v>   # ~/.gitconfig
git config --local <k> <v>    # .git/config
```

Precedence: local > global > system.

### Safety settings (recommended)

```bash
git config --global push.default simple
git config --global push.recurseSubmodules check
git config --global push.followTags true

git config --global fetch.prune true
git config --global fetch.fsckObjects true

git config --global pull.rebase true
# alternative (stricter):
# git config --global pull.ff only

git config --global rebase.autoStash true
git config --global rebase.autoSquash true

git config --global rerere.enabled true
git config --global rerere.autoUpdate true

git config --global init.defaultBranch main
```

Line endings (choose per platform/team):

```bash
# macOS/Linux:
git config --global core.autocrlf input
# Windows:
git config --global core.autocrlf true
```

### Quality-of-life settings

```bash
git config --global color.ui auto
git config --global diff.algorithm histogram
git config --global diff.renames true
git config --global core.pager "less -FRX"
```

Useful aliases (examples):

```bash
git config --global alias.s "status --short"
git config --global alias.lg "log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
git config --global alias.undo "reset --soft HEAD~1"
```

### Performance settings (large repos)

```bash
git config --global core.preloadindex true
git config --global core.untrackedCache true
git config --global pack.threads 0
git config --global gc.auto 256

git config --global feature.manyFiles true
git config --global core.commitGraph true
git config --global gc.writeCommitGraph true
```

### Review current configuration

```bash
git config --list --show-origin
git config --global --list
git config --local --list
```

## Examples

### Example: Minimal safe baseline

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global push.default simple
git config --global fetch.prune true
git config --global pull.rebase true
git config --global init.defaultBranch main
```

## Best Practices

**Do**:

- Apply safety defaults globally, repo-specific overrides locally
- Enable `rerere` to reduce repeated conflict resolution
- Keep aliases short but memorable; document team-standard aliases

**Don’t**:

- Set `pull.rebase` or `merge.ff` team-wide without agreement
- Add many experimental flags unless you’ve measured benefit
