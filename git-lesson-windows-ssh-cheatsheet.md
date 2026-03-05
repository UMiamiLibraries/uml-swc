# Git Setup & SSH Keys on Windows — Instructor Cheatsheet
> Software Carpentry Git Lesson | [swcarpentry.github.io/git-novice](https://swcarpentry.github.io/git-novice/)
> **Audience:** Instructors | **Shell:** Git Bash (always — not CMD or PowerShell)

---

## ⚠️ Before You Start

- Confirm students are using **Git Bash**, not CMD or PowerShell
- Check if the laptop is **university-managed** — security policies may block SSH or restrict `~/.ssh`
- Watch for **OneDrive** redirecting the home directory (common on Windows — see troubleshooting below)

---

## 1. Git Global Configuration

Run in Git Bash:

```bash
git config --global user.name "Firstname Lastname"
git config --global user.email "you@example.com"
git config --global core.editor "nano -w"    # or: notepad, code --wait
git config --global init.defaultBranch main
```

Verify:
```bash
git config --list
```

---

## 2. Check Where Home (`~`) Is

This is the **most common Windows problem**. Run:

```bash
echo $HOME
ls ~/.ssh
```

Expected: `C:/Users/username`
⚠️ **Red flag:** If it shows a OneDrive path like `C:/Users/username/OneDrive/Documents` — see [OneDrive Fix](#onedrive-fix) below.

---

## 3. Generate an SSH Key

Run in Git Bash:

```bash
ssh-keygen -t ed25519 -C "you@example.com"
```

At the prompts:
- **File location:** Press Enter to accept the default (`~/.ssh/id_ed25519`)
  — If default path looks wrong, type: `/c/Users/username/.ssh/id_ed25519`
- **Passphrase:** Optional but recommended; students can press Enter to skip for workshop ease

Verify keys were created:
```bash
ls ~/.ssh
# Should see: id_ed25519  id_ed25519.pub
```

---

## 4. Start ssh-agent and Add the Key

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

If that path fails, try the full Windows path:
```bash
ssh-add /c/Users/username/.ssh/id_ed25519
```

---

## 5. Copy the Public Key

```bash
cat ~/.ssh/id_ed25519.pub
```

Select and copy the entire output line (starts with `ssh-ed25519`, ends with the email).

**Or** use clip to copy directly to clipboard:
```bash
cat ~/.ssh/id_ed25519.pub | clip
```

---

## 6. Add Key to GitHub

1. GitHub → **Settings** → **SSH and GPG keys** → **New SSH key**
2. Title: something recognizable (e.g., `Workshop Laptop`)
3. Key type: **Authentication Key**
4. Paste the public key → **Add SSH key**

---

## 7. Test the Connection

```bash
ssh -T git@github.com
```

Expected response:
```
Hi username! You've successfully authenticated...
```

If it hangs or times out — see [SSH Blocked by Firewall](#ssh-blocked-by-firewall) below.

---

## Troubleshooting

### OneDrive Fix
OneDrive can redirect `Documents` or even the home directory, causing Git Bash to look for `.ssh` in the wrong place.

**Check it:**
```bash
echo $HOME
```

**Fix — Option A:** Explicitly use the correct path every time:
```bash
ssh-keygen -t ed25519 -C "email" -f /c/Users/username/.ssh/id_ed25519
```

**Fix — Option B:** Set `HOME` in Git Bash for the session:
```bash
export HOME=/c/Users/username
```
To make it permanent, add that line to `~/.bashrc`.

---

### `.ssh` Directory Doesn't Exist

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
```

---

### Wrong File Permissions (rare but can block SSH)

```bash
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub
```

---

### SSH Blocked by Firewall

University networks sometimes block port 22. Try SSH over HTTPS (port 443):

```bash
ssh -T -p 443 git@ssh.github.com
```

If this works, create `~/.ssh/config`:
```
Host github.com
  Hostname ssh.github.com
  Port 443
  User git
```

Then retry: `ssh -T git@github.com`

---

### Key Not Found / Wrong Path

If `ssh-add` throws `No such file or directory`:

```bash
# Find where the key actually ended up
find /c/Users/username -name "id_ed25519" 2>/dev/null
```

---

### University Security Blocking Key Generation

Some managed laptops restrict `.ssh` creation or key permissions.
**Fallback:** Use HTTPS with a GitHub Personal Access Token (PAT) instead of SSH. Not ideal for the lesson flow, but functional.

---

### Student Used Wrong Shell (CMD/PowerShell)

Signs: paths use `\`, commands like `ls` fail, `$HOME` doesn't work.
**Fix:** Close the terminal, reopen **Git Bash**, start from Step 2.

---

## Quick Reference — Common Paths in Git Bash

| Concept | Git Bash Path |
|---|---|
| Home directory | `~` or `/c/Users/username` |
| SSH folder | `~/.ssh` |
| Private key | `~/.ssh/id_ed25519` |
| Public key | `~/.ssh/id_ed25519.pub` |
| Git config file | `~/.gitconfig` |

---

## Useful One-Liners

```bash
# Check Git version
git --version

# Show all global config
git config --list --global

# Check if ssh-agent is running
ps aux | grep ssh-agent

# List loaded SSH keys
ssh-add -l

# Verbose SSH test (good for debugging)
ssh -vT git@github.com
```

---

*Last updated: March 2026*
