---
name: rebase-branch-onto-main
description: "Rebase current branch onto origin/main to remove previously merged changes from an ADO PR. Use when: PR shows old merged commits, branch is behind main, previously merged PR changes appear in new PR, need to sync branch with main before raising a PR."
argument-hint: "Optional: branch name to rebase (defaults to current branch)"
---

# Rebase Branch onto Origin/Main

## When to Use

- You have raised a PR for a branch and it was merged to `main` in ADO
- You continued working on the same branch (or a branch forked from it)
- When you raise a new PR, it shows the previously merged commits as part of the diff
- The root cause is that the local branch is behind `origin/main` and needs rebasing

## Procedure

### Step 1 — Check current branch and status

```powershell
git status
git branch --show-current
```

Confirm there are no uncommitted changes. If there are, stash them first:

```powershell
git stash push -m "wip before rebase"
```

### Step 2 — Fetch latest from origin

```powershell
git fetch origin
```

This updates the remote-tracking refs without modifying local branches.

### Step 3 — Verify how far behind main you are

```powershell
git log --oneline HEAD..origin/main
```

If this shows commits, your branch is behind `origin/main` — confirming the rebase is needed.

### Step 4 — Rebase onto origin/main

```powershell
git rebase origin/main
```

This replays your local commits on top of the latest `origin/main`, removing any commits that were already merged.

### Step 5 — Resolve conflicts (if any)

If `git rebase` pauses with conflicts:

1. Open each conflicted file and resolve manually
2. Stage the resolved files:
   ```powershell
   git add <file>
   ```
3. Continue the rebase:
   ```powershell
   git rebase --continue
   ```

To abort and return to the state before rebase:
```powershell
git rebase --abort
```

### Step 6 — Force push the rebased branch

Because rebase rewrites history, a force push is required:

```powershell
git push origin HEAD --force-with-lease
```

> `--force-with-lease` is safer than `--force` — it will fail if someone else has pushed to the same branch since your last fetch, preventing accidental overwrites.

### Step 7 — Restore stashed changes (if applicable)

If you stashed changes in Step 1:

```powershell
git stash pop
```

### Step 8 — Verify the PR diff in ADO

After force-pushing, refresh the PR in ADO. The previously merged commits should no longer appear in the diff — only your new changes should be visible.

## Quick Reference (no conflicts)

```powershell
git fetch origin
git rebase origin/main
git push origin HEAD --force-with-lease
```

## Troubleshooting

| Symptom | Cause | Fix |
|---|---|---|
| PR still shows old commits after push | ADO PR diff cache | Wait a moment and refresh |
| `git push` rejected | Someone else pushed to branch | Re-fetch and rebase again |
| Conflicts during rebase | Diverged changes | Resolve per Step 5 |
| Rebase moves wrong commits | Branch forked from stale point | Ensure `git fetch origin` ran first |