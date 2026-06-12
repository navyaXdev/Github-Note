---
tags:
  - git/branching/merging
---

# Merging Branches

When you finish working on a feature branch, you bring those changes back into the main branch. This is called **merging**.

**Basic merge command** (always run from the branch you want to merge INTO):

```bash
git switch master
git merge <branch-name>
```

---

## Types of Merge

| Merge Type | Merge Commit? | When to Use | Command |
|---|---|---|---|
| **Fast Forward** | ❌ No | Feature branch only, main untouched | `git merge feature` |
| **Three-Way** | ✅ Yes | Both branches have new commits | `git merge feature -m "msg"` |
| **Squash** | ✅ One clean commit | Clean history, hide messy commits | `git merge --squash feature` |
| **Octopus** | ✅ Yes (multi-parent) | Merge 3+ independent branches at once | `git merge a b c -m "msg"` |

---

## 1. Fast Forward Merge

Happens when **master has no new commits** since the branch was created. Git simply moves the master pointer forward — no merge commit needed.

```
Before:
master:  A ── B
               \
feature:        C ── D

After fast-forward:
master:  A ── B ── C ── D
                        ↑ (HEAD -> master, feature)
```

```bash
$ git switch master
$ git merge feature
Updating fae9976..c5343e3
Fast-forward
 feature.txt | 1 +
```

> 💡 The log stays **clean and linear** — looks like everything happened on one branch.

To **force a merge commit** even when fast-forward is possible:
```bash
git merge --no-ff feature -m "merge feature branch"
```

---

## 2. Three-Way Merge

Happens when **both branches have diverged** — master has new commits AND the feature branch has new commits. Git creates a **new merge commit** that combines both.

```
Before:
master:  A ── B ── C
               \
feature:        D ── E

After three-way merge:
master:  A ── B ── C ──── M  (merge commit)
               \         /
feature:        D ── E ──
```

```bash
$ git switch master
$ git merge login-feature -m "three way merge complete"
Merge made by the 'ort' strategy.

$ git log --oneline --graph --all
*   dc01b50 (HEAD -> master) three way merge complete
|\
| * 1cdba21 (login-feature) Login Feature added
* | 60efb0e Main update added
|/
* 47ec6b1 first commit
```

> 💡 The merge commit `M` has **two parents** — one from master, one from the feature branch.

---

## 3. Squash Merge

Takes **all commits from the feature branch** and squashes them into a **single staged change** on master. You then commit it as one clean commit.

```
Before:
feature:  A ── B ── C ── D ── E  (5 messy commits)

After squash merge to master:
master:   ... ── X  (one clean commit with all changes)
```

```bash
$ git switch master
$ git merge --squash ui-feature
Squash commit -- not updating HEAD
 footer.txt  | 1 +
 navbar.txt  | 1 +
 sidebar.txt | 1 +

$ git status
Changes to be committed:
        new file: footer.txt
        new file: navbar.txt
        new file: sidebar.txt

$ git commit -m "UI Feature Complete"   # ← you write the clean commit message
```

```bash
$ git log --oneline
cc333c1 (HEAD -> master) UI Feature Complete   # ← just ONE clean commit
dc01b50 three way merge complete
```

> 💡 The feature branch's individual commits **do not appear** in master's history.  
> ✅ Great for keeping master history clean when feature branches have lots of "WIP" and "fix typo" commits.

---

## 4. Octopus Merge

Merges **three or more branches** simultaneously in a single operation. Useful for integrating multiple independent feature branches at once.

```bash
$ git switch master
$ git merge feature-a feature-b feature-c -m "octopus merge all features"
Merge made by the 'octopus' strategy.

$ git log --oneline --graph --all
*-.   2203b98 (HEAD -> master) octopus merge all features
|\ \
| | * 09c56c9 (feature-c) feature c
| * | aee4383 (feature-b) feature b
* /  837c172 (feature-a) feature a
|/
* cc333c1 previous commit
```

> ⚠️ Octopus merge **cannot handle conflicts** between branches. If any conflicts exist, it will refuse and you'll need to merge branches individually.

---

## Viewing the Merge Graph

```bash
git log --oneline --graph --all
```

This shows a visual ASCII tree of all branches and merges — very useful for understanding the repo structure.

---

## [[05 - Merge Conflicts]]
