---
tags:
  - git/merge_conflicts
---

# Merge Conflicts

## What is a Merge Conflict?

A merge conflict happens when **two branches modify the same part of the same file** — and Git can't automatically decide which version to keep. Git stops and asks **you** to resolve it manually.

```
master branch:   "Python"    ← changed app.txt to say Python
feature branch:  "React"     ← changed same line to say React

Git: "I don't know which one to keep — you decide!"
```

---

## When Do Conflicts Happen?

- Two developers edit the **same line** in the same file on different branches
- One developer **deletes a file** that another developer modified
- Both branches **rename the same file** differently

---

## Reproducing a Conflict

```bash
# Both branches edit the same line in app.txt

$ git log --oneline --graph --all
* 164ba78 (HEAD -> master) Python     ← master says "Python"
| * 71921b9 (feature) React           ← feature says "React"
|/
* 5efdf56 java

$ git merge feature
Auto-merging app.txt
CONFLICT (content): Merge conflict in app.txt
Automatic merge failed; fix conflicts and then commit the result.
```

Git enters a `MERGING` state — shown in the prompt as `(master|MERGING)`.

---

## Reading the Conflict Markers

Git rewrites the conflicted file with special markers showing both versions:

```
<<<<<<< HEAD
Python
=======
React
>>>>>>> feature
```

| Marker | Meaning |
|---|---|
| `<<<<<<< HEAD` | Start of YOUR version (current branch) |
| `=======` | Dividing line between the two versions |
| `>>>>>>> feature` | End of the INCOMING version (branch being merged) |

---

## Resolving the Conflict

You have 3 choices:

**Option 1 — Keep your version (master):**
```
Python
```

**Option 2 — Keep incoming version (feature):**
```
React
```

**Option 3 — Keep both / combine:**
```
Python
React
```

Delete all the conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`) and leave only the final content you want.

### In VS Code — Visual Conflict Resolution

VS Code shows conflict markers as clickable buttons:
- `Accept Current Change` — keep master version
- `Accept Incoming Change` — keep feature version
- `Accept Both Changes` — keep both
- `Compare Changes` — see a side-by-side diff

---

## After Resolving — Completing the Merge

```bash
# 1. Stage the resolved file
git add app.txt

# 2. Complete the merge with a commit
git commit -m "Merge feature: resolved conflict — kept React"
```

```bash
$ git log --oneline
119f58a (HEAD -> master) Merge feature: resolved conflict
164ba78 Python
71921b9 (feature) React
5efdf56 java
```

---

## Aborting a Merge

If you started a merge and want to **cancel it** — go back to the state before merge:

```bash
git merge --abort
```

This works at any point during the conflict resolution as long as you haven't committed yet.

---

## Checking Which Files Have Conflicts

```bash
git status
```

```bash
$ git status
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")

Unmerged paths:
  (use "git add <file>..." to mark resolution)
        both modified: app.txt   ← conflicted file
```

---

## Preventing Conflicts — Best Practices

1. **Pull frequently** — keep your branch up to date with master
   ```bash
   git pull origin master
   ```
2. **Work in small commits** — easier to merge and less likely to conflict
3. **Communicate with teammates** — avoid two people editing the same file at the same time
4. **Use feature flags** — merge code into master even if incomplete, just hide it behind a flag
5. **Short-lived branches** — the longer a branch lives, the more it diverges from master

---

## [[06 - SSH and SSH setup]]
