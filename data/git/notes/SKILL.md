---
name: skill-git-notes
description: Master-level expertise in git notes for attaching metadata to commits without rewriting history. Focuses on namespaced refs, safe usage, syncing, and AI agent metadata patterns.
allowed-tools: Read
---

# Git Notes - Metadata Without Changing Commits

## Overview

Git notes let you attach additional metadata to Git objects (usually commits) without modifying those objects. Notes are stored in refs like `refs/notes/commits` (or custom refs you choose) and can be pushed/fetched like other refs.

This skill focuses on safe, practical notes usage: namespaced refs, structured metadata for tooling/agents, and collaboration-friendly syncing and merge strategies.

## Requirements

- Git v2.5.0+
- `jq` optional (only for querying structured JSON notes)

## Core Capabilities

- **Create, view, edit, append, remove** notes
- **Custom namespaces** with `--ref` to avoid conflicts
- **Structured metadata** (JSON/YAML) patterns
- **Configure display** (`notes.displayRef`) and rewrite behavior
- **Sync notes** with remotes (push/fetch specific refs)
- **Merge notes** safely (choose merge strategies)

## When to Use

**Use notes for**:

- AI agent metadata (actions taken, safety level, decisions)
- CI results, review annotations, deployment notes
- Supplemental context you don’t want in commit messages

**Don’t use notes for**:

- Secrets (notes are easy to share by accident)
- Things that must be visible to everyone by default (notes often require flags/config)

## Safety: Notes Still Modify the Repo

**Important**: `git notes add/edit/append/remove/merge/prune` create new commits in the notes ref. Treat them like any other repository-modifying operation.

## Integration with Other Skills

- `git-worker`: record operations only with explicit user permission
- `git-safety`: apply the same confirmation discipline to notes writes
- `first-principles`: decide what metadata matters and what is noise
- `simplicity`: keep schemas small; don’t create note sprawl

## Key Concepts

### Namespaces (Custom refs)

Prefer custom refs for isolation:

```bash
git notes --ref=ai-agent add -m "metadata" HEAD
git notes --ref=review add -m "LGTM" HEAD
```

Set a default notes ref for your shell/session:

```bash
export GIT_NOTES_REF=refs/notes/ai-agent
git notes add -m "metadata" HEAD
```

### Basic Operations

Add notes:

```bash
git notes add -m "This is a note"
git notes add -m "Note for commit" abc1234
git notes add -F note.txt abc1234
```

View notes:

```bash
git notes show
git notes show abc1234
git log --notes
git log --notes=ai-agent --notes=review
```

Edit / append:

```bash
git notes edit abc1234
git notes append -m "More info" abc1234
git notes add -f -m "Overwrite note" abc1234
```

Remove:

```bash
git notes remove
git notes remove abc1234
```

List:

```bash
git notes list
git notes get-ref
```

### Structured Data Pattern (AI metadata)

Use small, queryable JSON:

```bash
git notes --ref=ai-agent add -m '{
  "schema_version": "1.0",
  "timestamp": "2025-12-11T10:30:00Z",
  "agent": "skill-git-worker",
  "action": "worktree-add",
  "success": true,
  "details": {"path":"../feature-auth","branch":"feature/auth"}
}' HEAD
```

Query (optional):

```bash
git notes --ref=ai-agent show HEAD | jq -r '.action'
```

### Syncing Notes

Push a specific notes ref:

```bash
git push origin refs/notes/ai-agent
```

Fetch notes:

```bash
git fetch origin refs/notes/ai-agent:refs/notes/ai-agent
```

Fetch all notes (use intentionally):

```bash
git fetch origin '+refs/notes/*:refs/notes/*'
```

### Merging Notes

```bash
git fetch origin
git notes --ref=ai-agent merge origin/notes/ai-agent
```

Strategies:

```bash
git notes merge -s ours origin/notes/ai-agent
git notes merge -s theirs origin/notes/ai-agent
git notes merge -s union origin/notes/ai-agent
git notes merge -s cat_sort_uniq origin/notes/ai-agent
```

### Display Configuration

Show selected notes by default:

```bash
git config notes.displayRef refs/notes/ai-agent
git config --add notes.displayRef refs/notes/review
```

Preserve notes during rewrite operations (opt-in):

```bash
git config notes.rewrite.rebase true
git config notes.rewrite.amend true
```

## Examples

### Example 1: Record a CI run result on HEAD

```bash
git notes --ref=ci add -m '{"timestamp":"2025-12-11T10:30:00Z","tests_passed":142,"tests_failed":0}' HEAD
git push origin refs/notes/ci
```

### Example 2: Show review notes in log

```bash
git log --notes=review --oneline
```

## Best Practices

**Do**:

- Use namespaced refs (`--ref=...`) rather than default notes
- Keep notes concise (metadata, not full documentation)
- Version your schema (`schema_version`)
- Be explicit about syncing: push/fetch only the refs you intend

**Don’t**:

- Store secrets in notes
- Configure `notes.displayRef` to show everything (`refs/notes/*`)
- Assume other contributors will see notes unless configured

## Common Pitfalls

- Notes “missing” in `git log`: use `git log --notes` or configure `notes.displayRef`
- Notes not syncing: you must push/fetch notes refs explicitly
- Conflicting notes from multiple authors: pick a merge strategy and keep schemas stable
