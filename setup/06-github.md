# **itsRIGHTtime VPS Git & GitHub Setup**

## **Objective**

* Enable secure Git access for deployments.
* Configure global Git settings.
* Allow SSH-based connections to GitHub for DEV workflow.

---

## **1. Install Git on VPS**

```bash
sudo apt update
sudo apt install git -y
```

* Installs the latest stable Git version.
* Required for version control and deployments.

Check version:

```bash
git --version
```

---

## **2. Configure Global Git Settings**

Set your identity for commits:

```bash
git config --global user.name "Danishan"
git config --global user.email "danishan089@gmail.com"
git config --global init.defaultBranch main
```

* Ensures commits are correctly attributed.
* Sets default branch to `main` instead of `master`.

Verify:

```bash
git config --list
```

---

## **3. SSH Key for GitHub (Server Side)**

### a) Generate SSH Key

On VPS as `danishan`:

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh

ssh-keygen -t ed25519 -C "danishan089@gmail.com" -f ~/.ssh/irt_github
```

* `-f ~/.ssh/irt_github`: Names the key `irt_github`.
* Press Enter for no passphrase or optionally set one for extra security.

### b) Start SSH Agent & Add Key

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/irt_github
```

### c) Configure SSH for GitHub

Create `~/.ssh/config`:

```bash
nano ~/.ssh/config
```

Add:

```text
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/irt_github
    IdentitiesOnly yes
```

**Why:** Ensures this key is used for GitHub authentication.

---

## **4. Add SSH Key to GitHub Account**

1. Display public key:

```bash
cat ~/.ssh/irt_github.pub
```

2. Copy the content.
3. Go to **GitHub → Settings → SSH and GPG keys → New SSH key**.
4. Paste the key, give it a descriptive title (e.g., `itsRIGHTtime VPS Key`).

Test connection:

```bash
ssh -T git@github.com
```

* Should return:

```
Hi Danishan! You've successfully authenticated, but GitHub does not provide shell access.
```

---

## **5. Clone Repository Using SSH**

```bash
git clone git@github.com:username/repo.git /opt/irt-dev/projects/web-development/project1
```

* Replace `username/repo.git` with your GitHub repo.
* Directory structure follows your DEV project plan.

---

## **6. Local Machine Configuration**

### a) Generate SSH Key (if not already done)

```bash
ssh-keygen -t ed25519 -C "danishan089@gmail.com" -f ~/.ssh/irt_github_local
```

### b) Add Key to Local SSH Agent

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/irt_github_local
```

### c) Configure `~/.ssh/config`

```text
Host github
    HostName github.com
    User git
    IdentityFile ~/.ssh/irt_github_local
    IdentitiesOnly yes

Host itsrighttime-vps
    HostName itsrighttime.group
    User danishan
    IdentityFile ~/.ssh/id_ed25519
    Port 22
```

* `Host github`: Use `ssh github` to connect to GitHub via SSH.
* `Host itsrighttime-vps`: Alias for VPS login.

---

## **7. Optional Git Enhancements**

* **Auto-Correct Line Endings** for cross-platform DEV:

```bash
git config --global core.autocrlf input
```

* **Enable Credential Caching** (for HTTPS if needed):

```bash
git config --global credential.helper cache
```

* **Set Default Pull Behavior**:

```bash
git config --global pull.rebase false
```

---

## **Outcome**

* VPS is ready to securely pull/push to GitHub using SSH.
* Local machine is configured for easy SSH access to VPS and GitHub.
* Follows **production-ready security practices** for version control.
* Supports deployment of DEV projects under `/opt/irt-dev/`.


