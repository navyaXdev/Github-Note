---
tags:
  - git/basic
---

# Git — Basic Usage

## What is Git?

Git is a **distributed version control system** — it tracks changes to your files over time so you can go back to any previous state, collaborate with others, and manage your code history.

> 💡 Think of Git like a save system in a video game — you can save at any point and go back to any earlier save.

---

## Git Setup — First Time Only

Before using Git, tell it who you are. This info is attached to every commit you make.

```bash
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
```

Check your config anytime:
```bash
git config --list
```

---

## The 3 Areas of Git

Understanding these areas is key to understanding Git:

```
Working Directory  →  Staging Area  →  Repository (.git)
  (your files)         (git add)         (git commit)
```

| Area | Description |
|---|---|
| **Working Directory** | Your actual project files — where you edit code |
| **Staging Area** | A "preparation zone" — files you've marked to include in the next commit |
| **Repository** | The permanent history — all commits saved in the `.git` folder |

---

## Starting a Git Project

```bash
git init
```

Run this inside your project folder. It creates a hidden `.git` folder that starts tracking the directory.

```bash
dines@Revenge MINGW64 /d/Githubs/git_temp_repo (master)
$ git init
Initialized empty Git repository in D:/Githubs/git_temp_repo/.git/
```

---

## Checking Status

```bash
git status
```

Shows the current state of your working directory and staging area.

```bash
$ git status
On branch master
No commits yet
nothing to commit (create/copy files and use "git add" to track)
```

After creating a new file `app.txt`:

```bash
$ git status
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        app.txt
```

Git sees the file but isn't tracking it yet — it's **untracked**.

---

## Adding Files to Staging Area

```bash
git add <filename>          # specific file
git add file.txt file2.txt  # multiple files
git add .                   # all changed files
```

After `git add app.txt`:

```bash
$ git status
Changes to be committed:
        new file: app.txt
```

The file is now **staged** — ready to be committed.

---

## Committing

Save the staged changes permanently to the repository.

```bash
git commit -m "your commit message here"
```

```bash
$ git commit -m "first commit"
[master (root-commit) 48913d9] first commit
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 app.txt
```

> ✅ Write clear, meaningful commit messages — your future self will thank you.  
> Good: `"Add login validation to user form"`  
> Bad: `"fix stuff"` or `"aaa"`

**Shortcut** — skip `git add` for already-tracked files:
```bash
git commit -am "message"   # stages + commits tracked files in one step
```

---

## Viewing Commit History — `git log`

```bash
git log             # full detailed log
git log --oneline   # compact one-line per commit
git log --oneline --graph --all   # visual branch graph
```

```bash
$ git log --oneline
3da9d4a (HEAD -> master) I modify the app.txt
48913d9 first commit
```

- **HEAD** → points to the commit you're currently on
- The long string (`3da9d4a`) is the **commit hash** — a unique ID for each commit

---

## Seeing What Changed — `git diff`

Shows the **exact changes** in your files compared to the last commit (before staging).

```bash
git diff              # changes not yet staged
git diff --staged     # changes already staged
git diff <commit1> <commit2>   # compare two commits
```

```bash
$ git diff
-Hello Guys my name is Dinesh Patra.
+Hello Guys my name is Dinesh.
+I am 18 year old.
```

- Lines starting with `-` = removed
- Lines starting with `+` = added

---

## Full Workflow Summary

```bash
# 1. Initialize
git init

# 2. Check what's happening
git status

# 3. Stage your changes
git add .

# 4. Save to history
git commit -m "describe what you did"

# 5. Review history
git log --oneline
```

---

## [[02 - WORKING WITH FILES]]
