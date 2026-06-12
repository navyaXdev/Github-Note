---
tags:
  - git/branching
---

# Branching

## What is a Branch?

A branch is an **independent line of development** — a separate copy of your project where you can work freely without affecting the main codebase.

> 💡 The `master` (or `main`) branch is your **stable, production-ready** code. Never break it.

---

## Why Use Branches?

```
master branch (stable)
A ── B ── C ── D
              \
               E ── F   (backend team working here)

When done, merge back:
A ── B ── C ── D ─────── G  (merged)
              \         /
               E ── F
```

- The `master` branch stays **clean and working** at all times
- Developers experiment safely on their own branches
- If a feature breaks, it doesn't affect anyone else
- Multiple teams can work **in parallel** without interfering

**Common branch names in teams:**
- `main` / `master` — stable production code
- `develop` — integration branch for features
- `feature/login` — specific feature work
- `bugfix/header-crash` — bug fixes
- `hotfix/security-patch` — urgent production fixes

---

## Branch Commands

### List Branches

```bash
git branch         # list local branches (* = current)
git branch -a      # list all branches including remote
```

```bash
$ git branch
  font-end
* master        # ← you are here
```

> ⚠️ `git branch` only shows branches after the **first commit** is made. An empty repo has no branches yet.

---

### Create a Branch

```bash
git branch <branch-name>
```

```bash
$ git branch feature-login
$ git branch
  feature-login
* master          # ← still on master
```

---

### Switch to a Branch

```bash
git switch <branch-name>       # modern (recommended)
git checkout <branch-name>     # old way (still works)
```

```bash
$ git switch feature-login
Switched to branch 'feature-login'

$ git branch
* feature-login   # ← now here
  master
```

---

### Create AND Switch in One Command

```bash
git switch -c <branch-name>       # modern
git checkout -b <branch-name>     # old way
```

```bash
$ git switch -c feature-payment
Switched to a new branch 'feature-payment'
```

---

### Working on a Branch

Everything you commit on a branch stays **isolated** from other branches.

```bash
# On feature branch — create a file
$ echo "<h1>Hello</h1>" > index.html
$ git add .
$ git commit -m "add index.html"

$ git log --oneline
a85894f (HEAD -> font-end) add index.html
9cdbc39 (master) first commit
```

Switch back to master — `index.html` **won't be there**:
```bash
$ git switch master
$ ls
file.txt        # index.html is NOT here — it only exists on font-end branch
```

---

### Delete a Branch

```bash
git branch -d <branch-name>    # safe delete (only if merged)
git branch -D <branch-name>    # force delete (even if unmerged)
```

**Rules:**
- Cannot delete the branch you're **currently on** — switch away first
- `-d` protects you from accidentally losing unmerged work
- `-D` permanently removes the branch and any unmerged commits

**Safe delete workflow:**
```bash
git switch master          # 1. switch away from the branch
git merge feature-login    # 2. merge its work into master
git branch -d feature-login  # 3. now safe to delete
```

```bash
$ git branch -d font-end
Deleted branch font-end (was a85894f).
```

---

## Renaming a Branch

```bash
git branch -m <old-name> <new-name>    # rename any branch
git branch -m <new-name>               # rename current branch
```

---

## Quick Reference

```bash
git branch                    # list branches
git branch <name>             # create branch
git switch <name>             # switch to branch
git switch -c <name>          # create + switch
git branch -d <name>          # delete (safe)
git branch -D <name>          # delete (force)
git branch -m <old> <new>     # rename
```

---

## [[04 - Merging Branches]]
