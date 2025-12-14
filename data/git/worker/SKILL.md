---
name: skill-git-worker
description: Git execution mode for running commands with comprehensive safety checks, pre-flight validation, and automatic backups. Implements five-level safety system from read-only to critical operations with protected branch enforcement and multi-level confirmations.
allowed-tools: Read
---

# Git Worker - Execution Mode

## Overview

Git Worker Mode is the **execution mode** for git operations, actively running git commands with comprehensive safety checks, pre-flight validation, and automatic backups. Unlike advisory modes that explain concepts, Worker Mode performs actual git operations while maintaining safety as the absolute top priority.

This skill implements a five-level safety system ranging from read-only operations to critical repository changes, with automatic backup creation, protected branch enforcement, and detailed verification reporting. Worker Mode is designed for executing complex workflows reliably while preventing data loss and providing clear recovery paths when operations fail.

Safety is the absolute top priority in Worker Mode - every operation includes appropriate safety measures, from simple verification checks to multi-level confirmations with explicit typing requirements for critical operations.

## Requirements

- Git v2.5.0+ (minimum version for worktree support)
- Repository write access for modification operations
- Explicit user permission for all repository-modifying operations

## Core Capabilities

- **Five-Level Safety System** - Operations classified from safe (read-only) to critical (potential data loss)
- **Pre-Flight Validation** - Comprehensive checks before execution including state, branch, and remote verification
- **Automatic Backups** - Branch, stash, and tag backups created before risky operations
- **Protected Branch Enforcement** - Detection and protection for main/master/develop branches
- **Multi-Level Confirmations** - Risk-appropriate confirmation requirements from none to explicit typing
- **Batch Operations** - Execute multiple operations with rollback on failure
- **Error Recovery** - Auto-recovery when safe, detailed guidance when manual intervention needed
- **Result Verification** - Post-execution verification with detailed reporting

## When to Use

**Use git-worker for**:

- Executing git commands with safety checks and validation
- Creating or removing worktrees with path conflict detection
- Rebasing branches with automatic backup creation
- Merging branches with conflict detection
- Cleaning up merged branches in batch operations
- Force pushing with impact analysis and team coordination
- Complex multi-step workflows requiring reliability
- Operations requiring automatic backup and recovery paths

**Don't use git-worker for**:

- Learning git concepts (use git-guide instead)
- Understanding git workflows (use git-guide instead)
- Planning operations without execution (use git-guide instead)
- Making assumptions about repository state (always verify first)

## Integration with Other Skills

- **first-principles**: Question whether complex git operations are necessary, consider simpler alternatives
- **simplicity**: Prefer straightforward workflows over complex multi-step processes
- **git-guide**: Transition to advisory mode when explanation needed before execution
- **git-worktree**: Execute worktree creation, removal, and management operations
- **git-submodule**: Execute submodule operations with safety checks
- **git-rebase**: Execute rebase workflows with backup and conflict handling
- **git-recovery**: Execute recovery operations using reflog when needed
- **git-safety**: Implement safety protocols in all execution paths

## Mode Activation

Worker Mode is activated by requests indicating execution intent:

**Trigger Phrases**:

- "Create..." / "Delete..." / "Remove..."
- "Rebase..." / "Merge..." / "Cherry-pick..."
- "Clean up..." / "Prune..." / "Execute..."
- "Run..." / "Perform..." / "Apply..."

**Examples**:

- "Create a worktree for feature/auth"
- "Rebase this branch onto main"
- "Clean up merged branches"
- "Run a health check on this repository"

## Safety Levels

Worker Mode implements five safety levels based on operation risk:

### Level 1: Safe Operations

**Risk**: Minimal - read-only or easily reversible

**Operations**: `git status`, `git log`, `git diff`, `git branch --list`, `git worktree list`, `git remote -v`, `git reflog`

**Safety Measures**:

- No confirmation required
- No pre-flight checks needed
- Immediate execution

### Level 2: Low-Risk Operations

**Risk**: Low - modifies working directory but not history

**Operations**: `git checkout`, `git worktree add`, `git stash`, `git branch` (create), `git fetch`

**Safety Measures**:

- Pre-flight check: verify repository state
- Auto-stash uncommitted changes if needed
- Single confirmation for batch operations
- Verify results after execution

**Example Workflow**:

```md
User: "Create a worktree for feature/auth"

Pre-flight checks:
- ✓ Repository is clean
- ✓ Branch feature/auth exists
- ✓ No conflicting worktree paths

Executing:
git worktree add ../feature-auth feature/auth

Verification:
✓ Worktree created successfully at: ../feature-auth
✓ Checked out to: feature/auth
```

### Level 3: Medium-Risk Operations

**Risk**: Medium - modifies local history or repository structure

**Operations**: `git commit`, `git rebase` (local), `git merge`, `git branch -d`, `git worktree remove`, `git clean -fd`

**Safety Measures**:

- Comprehensive pre-flight checks
- Protected branch detection
- Automatic backup (branch or stash)
- Explicit confirmation required
- Detailed result reporting

**Example Workflow**:

```md
User: "Rebase feature/auth onto main"

Pre-flight checks:
- ✓ Current branch: feature/auth
- ✓ No uncommitted changes
- ✓ Branch has not been pushed (safe to rebase)
- ✓ Target branch 'main' is up to date

Creating safety backup:
git branch backup/feature-auth-2025-12-14

Confirm operation:
About to rebase feature/auth onto main (15 commits)
Backup branch created: backup/feature-auth-2025-12-14
Proceed with rebase? [y/N]

Executing (after confirmation):
git rebase main

Verification:
✓ Rebase completed successfully
✓ 15 commits replayed
✓ No conflicts
✓ Backup available at: backup/feature-auth-2025-12-14
```

### Level 4: High-Risk Operations

**Risk**: High - modifies published history or irreversible changes

**Operations**: `git push --force`, `git reset --hard`, `git rebase` (published), `git branch -D`, `git clean -fdx`

**Safety Measures**:

- Multi-level confirmation required
- Detailed impact analysis
- Automatic backup creation
- Protected branch enforcement
- Clear warning messages
- Recovery instructions provided

**Example Workflow**:

```md
User: "Force push this branch"

Pre-flight checks:
- ✓ Current branch: feature/auth
- ⚠ Branch has been pushed to remote
- ⚠ This will rewrite published history

Protected branch check:
- ✓ Not a protected branch (main/master/develop)

Impact analysis:
- Remote commits that will be overwritten: 3
- Local commits to push: 5
- Other users may be affected if they pulled this branch

Creating safety backup:
git branch backup/feature-auth-force-push-2025-12-14

Multi-level confirmation:
⚠ WARNING: Force push will rewrite published history
⚠ Other team members working on this branch will need to reset
⚠ This operation cannot be easily undone on the remote

Are you absolutely sure you want to force push? Type 'force push' to confirm:

Executing (after explicit confirmation):
git push --force-with-lease origin feature/auth

Verification:
✓ Force push completed
✓ 5 commits pushed
✓ Backup available at: backup/feature-auth-force-push-2025-12-14

Recovery instructions:
If you need to undo this force push:
  git push --force origin backup/feature-auth-force-push-2025-12-14:feature/auth
```

### Level 5: Critical Operations

**Risk**: Critical - potential data loss or severe repository impact

**Operations**: `git push --force` to main/master, `git reset --hard` on shared branches, `git clean -fdx` with important files, `git gc --aggressive --prune=now`

**Safety Measures**:

- Multiple explicit confirmations
- Team notification recommended
- Full repository backup suggested
- Recovery plan provided before execution
- Requires typing exact operation name
- Cannot be batch-executed

**Example Workflow**:

```md
User: "Force push to main"

Critical operation warning:
🛑 CRITICAL: You are attempting to force push to 'main'
🛑 This will affect all team members
🛑 This may break CI/CD pipelines
🛑 This should only be done in emergency situations with team approval

Required actions before proceeding:
1. Notify all team members
2. Ensure no one else is pushing to main
3. Have a recovery plan
4. Consider alternatives (revert commits instead)

Alternative recommendation:
Instead of force pushing, consider:
  git revert <problematic-commits>
  git push origin main

Do you want to proceed with the safer alternative? [Y/n]

If user insists on force push:
Type the full branch name 'main' to confirm force push:

After typing 'main':
Type 'I UNDERSTAND THE RISKS' to proceed:

Executing (only after both confirmations):
[Proceeds with extreme caution]
```

## Pre-Flight Checks

Comprehensive validation before operation execution:

### Repository State Checks

**Working Directory Status**:

- Uncommitted changes detection
- Untracked files detection
- Merge/rebase in progress detection
- Conflict detection

**Branch Status**:

- Current branch identification
- Tracking branch status
- Ahead/behind remote status
- Protected branch detection

**Remote Status**:

- Remote connectivity check
- Fetch status (up to date?)
- Push/pull requirements

### Operation-Specific Checks

**For Rebase**:

- Uncommitted changes (will auto-stash if configured)
- Branch has not been pushed (or explicit override)
- Target branch exists and is up to date
- No ongoing rebase/merge

**For Force Push**:

- Branch protection status
- Remote existence
- Impact analysis (commits to be overwritten)
- Team notification requirements

**For Branch Deletion**:

- Branch merge status
- Unmerged commits detection
- Remote branch existence
- Backup creation

**For Worktree Operations**:

- Path conflicts
- Branch availability
- Submodule status
- Disk space (for new worktrees)

## Automatic Backups

Worker Mode creates backups before risky operations:

### Backup Types

**Branch Backups**:

```bash
git branch backup/<original-branch>-<operation>-<date>
```

Created before: Rebase, reset, force operations

**Stash Backups**:

```bash
git stash push -m "Auto-backup before <operation>"
```

Created before: Checkout with uncommitted changes, rebase with auto-stash, clean operations

**Tag Backups**:

```bash
git tag backup/<operation>-<date>
```

Created before: Major history rewrites, submodule operations, repository cleanup

### Backup Retention

- Backups are **never automatically deleted**
- User can manually clean up backups
- Backup branches clearly named with date and operation
- Recovery instructions always provided

## Execution Workflow

### Standard Execution Flow

1. **Parse Request** - Identify operation type, extract parameters, determine safety level
2. **Pre-Flight Checks** - Run all relevant checks, report issues, abort if critical issues found
3. **Safety Backup** - Create appropriate backup (if needed), verify backup creation
4. **Confirmation** - Present operation summary, show impact analysis, request explicit confirmation (if required)
5. **Execute Operation** - Run git command(s), monitor for errors, handle conflicts/issues
6. **Verification** - Verify operation completed successfully, check repository state, report detailed results
7. **Recovery Information** - Provide undo instructions, reference backup locations, link to relevant documentation

### Error Handling

When operations fail:

**Immediate Halt** - Stop execution immediately, preserve current state

**Error Analysis** - Identify error type, assess impact, determine recovery path

**Auto-Recovery** (when safe) - Restore from backup, reset to safe state, report actions taken

**User Guidance** - Explain what went wrong, provide recovery options, offer alternative approaches

**Example Error Recovery**:

```md
Executing rebase:
git rebase main

⚠ Rebase failed with conflicts in:
  - src/auth.js
  - src/config.js

Auto-recovery:
git rebase --abort
✓ Repository restored to pre-rebase state

Recovery options:
1. Resolve conflicts manually:
   git rebase main
   # Fix conflicts in src/auth.js and src/config.js
   git add <fixed-files>
   git rebase --continue

2. Use merge instead:
   git merge main

3. Restore from backup:
   git reset --hard backup/feature-auth-2025-12-14

Which approach would you prefer?
```

## Protected Branches

Worker Mode enforces protection for critical branches:

### Detection Methods

**Conventional Names**: main, master, develop, production, release/*, hotfix/*

**Remote Platform API** (when available): GitHub protected branches, GitLab protected branches, Bitbucket branch restrictions

**User Configuration**: Custom protection rules, project-specific settings

### Protection Levels

**Read-Only Operations**: Always allowed

**Standard Operations**: Allowed with confirmation (merges into protected branches, regular pushes)

**Restricted Operations**: Require elevated confirmation (force pushes, direct commits, branch deletion)

**Forbidden Operations**: Blocked entirely unless override (force push without --force-with-lease, rebase of published commits, history rewrites)

## Batch Operations

Execute multiple operations in sequence:

### Batch Execution Rules

**Safety First**:

- Each operation gets individual safety checks
- Failure of one operation halts the batch
- Rollback strategy for batch failures

**Confirmation Strategy**:

- Single confirmation for multiple low-risk operations
- Individual confirmation for each high-risk operation
- Batch summary before execution

**Progress Reporting**:

- Clear progress indicators
- Success/failure for each step
- Cumulative results

**Example Batch Operation**:

```md
User: "Clean up all merged branches"

Analyzing branches:
Found 12 merged branches:
  - feature/old-auth (merged 30 days ago)
  - feature/payment-flow (merged 15 days ago)
  - bugfix/login-issue (merged 7 days ago)
  [... 9 more]

Safety checks:
- ✓ All branches have been merged to main
- ✓ All branches have been pushed to remote
- ✓ Backups can be recovered from remote

Batch operation summary:
About to delete 12 local branches
Proceed? [y/N]

Executing:
[1/12] Deleting feature/old-auth... ✓
[2/12] Deleting feature/payment-flow... ✓
[3/12] Deleting bugfix/login-issue... ✓
...
[12/12] Deleting feature/xyz... ✓

Results:
✓ Successfully deleted 12 branches
✓ All branches still available on remote
✓ To recover: git checkout -b <branch-name> origin/<branch-name>
```

## Permission Model

**CRITICAL PRINCIPLE**: Worker Mode **NEVER** modifies the repository without explicit user permission.

### Operations Requiring Permission

**ALL** repository-modifying operations require permission:

- Creating/deleting branches
- Creating/removing worktrees
- Committing changes
- Rebasing/merging
- Pushing to remote
- Adding/editing git notes
- Any operation that changes .git/ contents

### Read-Only Operations (No Permission)

These operations don't require permission:

- `git status`, `git log`, `git diff`, `git show`
- `git branch --list`, `git notes show` (read-only)

### Permission Patterns

**Single operation**: "May I create a worktree at ../feature-auth?" → User confirms → Execute

**Batch operation**: "May I delete these 12 merged branches?" → User confirms → Execute all with progress

**Session-level**: "This workflow involves multiple operations. May I proceed with automatic execution for this session?" → User confirms → Execute all steps, reporting progress

## Git Notes for Operation Tracking (Optional)

Worker Mode **can optionally** use git notes to track operations, but **ONLY with explicit user permission**.

### Permission Required

Git notes modify the repository. Worker Mode **MUST**:

1. **Request permission** before using git notes
2. **Never assume permission** - each use requires approval or session-level opt-in
3. **Respect preferences** - Remember user choices for session or permanently

**Example Permission Request**:

```md
Worker Mode: "Operation complete. Would you like me to record metadata
             about this operation in git notes? This will create a commit
             in refs/notes/skill-git but won't affect your working directory
             or commit history."

User: "Yes" / "No" / "Always for this session" / "Never"
```

### What Gets Recorded (If Permitted)

```bash
git notes --ref=skill-git add -m '{
  "timestamp": "2025-12-14T10:30:00Z",
  "operation": "worktree-add",
  "safety_level": 2,
  "details": {
    "path": "../feature-auth",
    "branch": "feature/auth"
  },
  "user_approved": true
}' HEAD
```

### Benefits of Notes Tracking

- Audit trail of operations performed
- Context preservation across sessions
- Decision tracking for complex workflows
- Non-intrusive - doesn't modify commits or working directory

### When to Offer Notes

Only offer notes tracking for **significant operations**:

- ✅ Worktree creation/removal, rebases, merges, branch operations
- ❌ Simple read-only operations (status, log, diff)

## Examples

### Example 1: Creating a Worktree with Safety Checks

```bash
# User request
"Create a worktree for feature/authentication at ../auth"

# Worker Mode execution
Pre-flight checks:
- ✓ Repository clean
- ✓ Branch feature/authentication exists
- ✓ Path ../auth available
- ✓ No submodule conflicts

Executing:
git worktree add ../auth feature/authentication

Verification:
✓ Worktree created at: /Users/dev/project/../auth
✓ Branch: feature/authentication
✓ HEAD at: abc1234 (Add authentication module)

Next steps:
cd ../auth && git status
```

### Example 2: Batch Branch Cleanup with Rollback

```bash
# User request
"Clean up all merged feature branches"

# Worker Mode execution
Analyzing:
Found 8 merged branches:
  - feature/user-profile (merged 45 days ago)
  - feature/api-endpoint (merged 30 days ago)
  - feature/dashboard (merged 15 days ago)
  [... 5 more]

Safety checks:
- ✓ All merged to main
- ✓ All pushed to remote
- ✓ Can recover from origin if needed

Batch summary:
Delete 8 local branches? [y/N] y

Executing:
[1/8] feature/user-profile ✓
[2/8] feature/api-endpoint ✓
[3/8] feature/dashboard ✓
[4/8] feature/export ⚠ Failed (unmerged commits detected)

Halting batch operation.
Rolled back: 0 branches (all remote-recoverable)
Failed at: feature/export

Recovery:
7 branches remain unchanged
To retry without feature/export, exclude it from selection
```

### Example 3: Rebase with Conflict Recovery

```bash
# User request
"Rebase my feature branch onto latest main"

# Worker Mode execution
Pre-flight:
- ✓ Current branch: feature/new-ui
- ✓ No uncommitted changes
- ⚠ Branch has been pushed (will require force-push after rebase)
- ✓ main is up to date

Creating backup:
git branch backup/new-ui-rebase-2025-12-14

Confirmation:
Rebase feature/new-ui onto main (23 commits)
⚠ Will require force-push after completion
Backup: backup/new-ui-rebase-2025-12-14
Proceed? [y/N] y

Executing:
git rebase main

⚠ Conflict detected in src/components/Header.js

Options:
1. Resolve manually:
   # Edit src/components/Header.js
   git add src/components/Header.js
   git rebase --continue

2. Skip this commit:
   git rebase --skip

3. Abort and restore:
   git rebase --abort

Which option? [1/2/3]
```

## Best Practices

**Do**:

- ✅ Review pre-flight check results before confirming operations
- ✅ Create backups before risky operations (automatic, but verify they exist)
- ✅ Use batch operations for repetitive tasks (branch cleanup, worktree management)
- ✅ Verify repository state after operations complete
- ✅ Keep backup branches until you're confident operations succeeded
- ✅ Use --force-with-lease instead of --force for safer force pushes
- ✅ Request explanation if uncertain about operation safety level

**Don't**:

- ❌ Skip confirmations for high-risk operations
- ❌ Ignore pre-flight warnings about uncommitted changes
- ❌ Force push to protected branches (main/master/develop) without team coordination
- ❌ Delete backup branches immediately after operations
- ❌ Batch high-risk operations without reviewing each one
- ❌ Override safety checks without understanding consequences
- ❌ Use Worker Mode for learning (use git-guide instead)

## Common Pitfalls

**Pitfall 1: Ignoring pre-flight warnings**

- Pre-flight checks detect potential issues before execution
- Warnings about uncommitted changes, unpushed commits, or branch protection should not be dismissed
- Solution: Address warnings before proceeding, or acknowledge risks explicitly

**Pitfall 2: Not verifying backup creation**

- Automatic backups are created, but verification confirms they exist
- Check backup branch/tag exists before proceeding with risky operations
- Solution: Review backup location in pre-operation summary

**Pitfall 3: Force pushing without team coordination**

- Force pushes affect all collaborators on shared branches
- Level 4/5 operations require team awareness
- Solution: Notify team before force pushing shared branches, use --force-with-lease, consider alternatives like revert
