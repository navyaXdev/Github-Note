---
tags:
  - git/working_with_files
---

# Working With Files

---

## .gitignore — Excluding Files from Tracking

Sometimes you don't want Git to track certain files — passwords, API keys, log files, compiled output, etc.  
Create a file named **`.gitignore`** in your project root and list what to exclude.

```bash
# .gitignore

pass.txt          # ignore a specific file
tmp/              # ignore an entire folder
*.log             # ignore all .log files
temp*             # ignore files starting with "temp"
*.env             # ignore environment variable files
node_modules/     # ignore Node.js dependencies folder
__pycache__/      # ignore Python cache folder
```

After adding `.gitignore`:

```bash
$ git status
On branch master
Untracked files:
        .gitignore
# pass.txt is NOT shown — Git is ignoring it ✅
```

> ⚠️ `.gitignore` only ignores **untracked** files. If you already committed a file, adding it to `.gitignore` won't remove it from history — you need `git rm --cached <file>` first.

**Commit your `.gitignore` file** so teammates also ignore the same files.

---

## .gitkeep — Tracking Empty Folders

Git cannot track empty folders — it only tracks files.  
If you need an empty folder in your repo (e.g. `logs/`, `uploads/`), create a placeholder file inside it:

```bash
touch logs/.gitkeep
```

Then Git will track the folder through this file.  
`.gitkeep` is just a convention — the file has no special meaning to Git; it's simply a way to keep empty folders in version control.

---

## Undoing Changes

### The 3 Types of Reset

| Command | Removes Commit? | Unstages? | Removes File Changes? |
|---|---|---|---|
| `--soft HEAD~1` | ✅ Yes | ❌ No (stays staged) | ❌ No |
| `HEAD~1` (mixed) | ✅ Yes | ✅ Yes (unstaged) | ❌ No |
| `--hard HEAD~1` | ✅ Yes | ✅ Yes | ✅ Yes (changes deleted) |

> `HEAD~1` = one commit back from current. `HEAD~2` = two commits back, etc.

---

### Soft Reset — Undo commit, keep changes staged

```bash
git reset --soft HEAD~1
```

The commit is removed, but your changes stay in the **staging area** — ready to re-commit with a different message.

```bash
$ git log --oneline
415b517 (HEAD -> master) I add my information   # ← want to undo this
3da9d4a I modify the app.txt
48913d9 first commit

$ git reset --soft HEAD~1

$ git log --oneline
3da9d4a (HEAD -> master) I modify the app.txt   # commit removed
48913d9 first commit

$ git status
Changes to be committed:       # ← changes still staged
        modified: app.txt
```

> 💡 Use soft reset when you just want to **fix a commit message** or **combine with other changes**.

---

### Mixed Reset (Default) — Undo commit and unstage

```bash
git reset HEAD~1
```

The commit is removed and changes are **unstaged** (back to working directory). Your file content is still there.

```bash
$ git reset HEAD~1
Unstaged changes after reset:
M       app.txt

$ git status
Changes not staged for commit:
        modified: app.txt    # ← still in your files, just not staged
```

Then to also discard the file changes:
```bash
git restore app.txt    # reverts file to last committed state
```

---

### Hard Reset — Undo commit AND delete changes permanently

```bash
git reset --hard HEAD~1
```

The commit is removed **and all file changes are permanently deleted**. Cannot be undone easily.

```bash
$ git reset --hard HEAD~1
HEAD is now at 48878c3 add ignore file

$ cat app.txt
Hello Guys my name is Dinesh Patra.   # all new content is gone
```

> ⚠️ **Danger zone!** Hard reset permanently destroys uncommitted work. Only use when you're 100% sure you want to discard changes.

---

### Unstaging a File

Move a file from staging area back to working directory (without deleting changes):

```bash
git restore --staged <filename>
```

```bash
$ git restore --staged app.txt
# app.txt is now unstaged but changes are still in the file
```

### Discarding File Changes

Revert a file back to how it was in the last commit (deletes your edits):

```bash
git restore <filename>
```

```bash
$ git restore app.txt
# All unsaved edits to app.txt are gone — file is back to last commit state
```

---

## Reverting a Commit — Safe Undo for Shared History

`git reset` rewrites history — **dangerous on shared/remote branches**.  
`git revert` is the **safe way** to undo a commit because it creates a **new commit** that cancels the old one — history is preserved.

```bash
git revert <commit-hash>
```

```bash
$ git log --oneline
accab49 add 3
6b610e4 add 2     # ← want to undo this
ae3cafa add 1

$ git revert 6b610e4
# Git creates a NEW commit that undoes the changes from 6b610e4

$ git log --oneline
84c0bd5 Revert "add 2"    # new commit added
accab49 add 3
6b610e4 add 2             # original commit still in history
ae3cafa add 1
```

If a conflict occurs during revert:
1. Resolve the conflict in the file
2. `git add .`
3. `git revert --continue`

To cancel a revert in progress:
```bash
git revert --abort
```

---

### Reset vs Revert — When to Use Which?

| Situation | Use |
|---|---|
| Undo on **local** branch, not pushed yet | `git reset` |
| Undo on a **shared/remote** branch (pushed) | `git revert` |
| Want to keep full history | `git revert` |
| Want clean history, no trace | `git reset` |

---

## [[03 - BRANCHING]]
