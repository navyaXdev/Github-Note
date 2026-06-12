---
tags:
  - git/remote
---

# Push and Pull

## What is a Remote?

A **remote** is a version of your repository hosted on the internet (like GitHub, GitLab, Bitbucket).  
`origin` is the default name Git gives to your remote repository.

```bash
# Add a remote (after git init on a new project)
git remote add origin git@github.com:username/repo.git

# Check your remotes
git remote -v
```

---

## git push — Upload Your Changes

After making commits locally, use `git push` to upload them to GitHub.

```bash
# First time — set the upstream tracking branch
git push -u origin main

# Every time after that (Git remembers the upstream)
git push
```

- `-u` = `--set-upstream` — links your local branch to the remote branch so future `git push` / `git pull` work without specifying the branch name

```bash
$ git push -u origin master
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Writing objects: 100% (3/3), 280 bytes | 280.00 KiB/s, done.
Branch 'master' set up to track remote branch 'master' from 'origin'.
```

### Pushing a New Branch

```bash
git push -u origin feature-login    # push a branch that doesn't exist on remote yet
```

### Force Push (use with caution!)

```bash
git push --force    # overwrites remote history — dangerous on shared branches!
```

> ⚠️ Never force push to `main`/`master` on a shared repo — you'll overwrite teammates' work.

---

## git pull — Download Latest Changes

If the remote has been updated (teammate pushed, or you pushed from another computer), download those changes with `git pull`.

```bash
git pull
```

```bash
$ git pull
remote: Enumerating objects: 5, done.
Updating a1b2c3d..f3e2d1c
Fast-forward
 newfile.txt | 1 +
 1 file changed, 1 insertion(+)
```

`git pull` = `git fetch` + `git merge` in one command.

---

## git fetch vs git pull

| Command | Downloads? | Merges? | Safe? |
|---|---|---|---|
| `git fetch` | ✅ Yes | ❌ No | ✅ Very safe |
| `git pull` | ✅ Yes | ✅ Yes (auto) | ⚠️ Can cause conflicts |

### git fetch — Download without merging

```bash
git fetch origin           # download all remote changes
git log origin/master      # see what changed remotely
git diff origin/master     # compare your local with remote
git merge origin/master    # merge when you're ready
```

> 💡 Use `git fetch` when you want to **see what's changed** before merging it into your work.

### git pull — Download and merge immediately

```bash
git pull                   # fetches + merges in one step
git pull origin master     # explicit version
```

---

## Cloning a Repository

To get a copy of an existing GitHub repository:

```bash
git clone git@github.com:username/repo.git         # SSH (recommended)
git clone https://github.com/username/repo.git     # HTTPS
```

This downloads the full repo history and automatically sets up `origin`.

---

## Full Remote Workflow

```bash
# 1. Clone the repo (first time)
git clone git@github.com:username/project.git
cd project

# 2. Create a branch for your work
git switch -c feature-login

# 3. Make changes and commit
git add .
git commit -m "add login page"

# 4. Push your branch to GitHub
git push -u origin feature-login

# 5. Teammate pushed to master — pull their changes
git switch master
git pull

# 6. After your branch is approved and merged on GitHub
git branch -d feature-login   # clean up local branch
```

---

## Common Errors and Fixes

```bash
# Error: "rejected — non-fast-forward"
# Someone else pushed before you — pull first
git pull
git push

# Error: "src refspec main does not match any"
# You're using wrong branch name
git branch        # check your branch name
git push -u origin master   # use actual branch name

# Error: "permission denied (publickey)"
# SSH key not set up or not added to agent
ssh-add ~/.ssh/id_ed25519
```

---

## Quick Reference

```bash
git remote add origin <url>    # connect local repo to GitHub
git remote -v                  # check remote URLs
git push -u origin main        # first push (sets upstream)
git push                       # push (after upstream set)
git pull                       # fetch + merge latest
git fetch                      # download only, no merge
git clone <url>                # copy a repo from GitHub
```
