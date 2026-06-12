---
tags:
  - git/ssh
---

# SSH and SSH Setup

## What is SSH?

**SSH (Secure Shell)** is a protocol for secure communication between your computer and a remote server (like GitHub).

Instead of typing your GitHub username and password every time you push/pull, SSH uses a **key pair** for automatic, secure authentication.

---

## How SSH Keys Work

SSH generates **two keys** — a matched pair:

| Key | File | Rule |
|---|---|---|
| **Private Key** | `id_ed25519` | Stays on YOUR computer — **never share this** |
| **Public Key** | `id_ed25519.pub` | Goes to GitHub — safe to share |

> 💡 Think of it like a padlock and key: you give GitHub the padlock (public key), you keep the key (private key). Only your key can open GitHub's padlock.

---

## Step 1 — Generate SSH Key Pair

```bash
ssh-keygen -t ed25519 -C "your@email.com"
```

- `-t ed25519` — the encryption algorithm (modern, recommended)
- `-C` — a label/comment to identify the key (use your email)

You'll be prompted for:
- **File location** — press Enter to use the default (`~/.ssh/id_ed25519`)
- **Passphrase** — optional extra security (recommended), or press Enter to skip

```bash
Generating public/private ed25519 key pair.
Enter file in which to save the key (/Users/you/.ssh/id_ed25519):
Enter passphrase (empty for no passphrase):
Your identification has been saved in /Users/you/.ssh/id_ed25519
Your public key has been saved in /Users/you/.ssh/id_ed25519.pub
```

Two files are now created in your `~/.ssh/` folder:
- `id_ed25519` — your **private key** (never share this)
- `id_ed25519.pub` — your **public key** (this goes to GitHub)

---

## Step 2 — Start the SSH Agent

The SSH agent runs in the background and manages your keys so you don't re-enter your passphrase repeatedly.

```bash
# Start the agent
eval "$(ssh-agent -s)"

# Output: Agent pid 12345
```

```bash
# Add your private key to the agent
ssh-add ~/.ssh/id_ed25519

# Output: Identity added: /Users/you/.ssh/id_ed25519
```

> On **Windows (Git Bash)**, this is the same. On **Mac**, you may also need `ssh-add --apple-use-keychain ~/.ssh/id_ed25519`.

---

## Step 3 — Copy Your Public Key

```bash
cat ~/.ssh/id_ed25519.pub
```

This outputs something like:
```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAA... your@email.com
```

Copy the **entire output** — from `ssh-ed25519` to your email.

---

## Step 4 — Add Key to GitHub

1. Go to **GitHub** → **Settings** → **SSH and GPG keys**
2. Click **New SSH key**
3. Give it a title (e.g. `My Laptop`)
4. Paste your public key into the **Key** field
5. Click **Add SSH key**

---

## Step 5 — Test the Connection

```bash
ssh -T git@github.com
```

If successful:
```
Hi your-username! You've successfully authenticated,
but GitHub does not provide shell access.
```

> If you see your GitHub username — SSH is working perfectly ✅

---

## Using SSH vs HTTPS

When cloning or adding a remote, make sure you use the **SSH URL** (not HTTPS):

```bash
# SSH URL (use this when SSH is set up)
git clone git@github.com:username/repo.git

# HTTPS URL (requires username/password or token)
git clone https://github.com/username/repo.git
```

To check/change an existing remote:
```bash
git remote -v                                          # check current URL
git remote set-url origin git@github.com:username/repo.git   # switch to SSH
```

---

## Troubleshooting

```bash
# Permission denied error?
ssh -vT git@github.com    # verbose mode shows exactly what's happening

# Key not found?
ssh-add -l                # list keys currently loaded in agent
ssh-add ~/.ssh/id_ed25519 # re-add the key

# Wrong email/key? Generate a new one and re-add to GitHub
```

---

## [[07 - Push and Pull]]
