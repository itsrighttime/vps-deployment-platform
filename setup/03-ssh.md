
# **itsRIGHTtime VPS SSH Setup (with Local Config)**

## **Objective:**

Securely configure SSH for your server, enforce key-based authentication, disable root login, harden security, and allow easy login from your local machine using IP or domain.

---

## **1. Create a New User on VPS**

```bash
sudo adduser danishan
sudo usermod -aG sudo danishan
```

**Why:** Avoid using root; the new user will have sudo privileges.

---

## **2. Set Up `.ssh` Directory on VPS**

```bash
su - danishan
mkdir -p ~/.ssh
chmod 700 ~/.ssh
```

---

## **3. Generate SSH Key Pair on Local Machine**

```bash
ssh-keygen -t ed25519 -C "danishan089@gmail.com"
```

* Generates **private key** (`id_ed25519`) and **public key** (`id_ed25519.pub`).
* Keep the private key secure.

---

## **4. Copy Public Key to VPS**

On VPS, as `danishan`:

```bash
nano ~/.ssh/authorized_keys
```

* Paste the **public key** from your local machine.
* Save file and set permissions:

```bash
chmod 600 ~/.ssh/authorized_keys
```

---

## **5. Harden SSH on VPS**

Edit `/etc/ssh/sshd_config`:

```
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys

LoginGraceTime 30
MaxAuthTries 4
ClientAliveInterval 300
ClientAliveCountMax 3
```

Restart SSH:

```bash
sudo systemctl restart sshd
```

---

## **6. Local Machine SSH Configuration**

Create or edit SSH config file:

```bash
nano ~/.ssh/config
```

Add the following entries:

```text
# Connect using domain
Host itsrighttime
    HostName itsrighttime.group
    User danishan
    IdentityFile ~/.ssh/id_ed25519
    Port 22

# Connect using IP
Host itsrighttime-ip
    HostName 72.60.201.63
    User danishan
    IdentityFile ~/.ssh/id_ed25519
    Port 22
```

**Notes:**

* `Host`: Custom nickname for easy SSH login (`ssh itsrighttime`).
* `HostName`: VPS domain or IP.
* `IdentityFile`: Path to your private key.
* `Port`: Default 22, or custom if you changed it.

Test connections:

```bash
ssh itsrighttime      # Connects using domain
ssh itsrighttime-ip   # Connects using IP
```

---

## **7. Optional Security Enhancements**

### a) Change SSH Port (e.g., 2222)

On VPS:

```bash
sudo nano /etc/ssh/sshd_config
Port 2222
sudo systemctl restart sshd
```

Update firewall:

```bash
sudo ufw allow 2222/tcp
sudo ufw delete allow 22
```

Update local `~/.ssh/config`:

```text
# Example for custom port
Host itsrighttime
    HostName itsrighttime.group
    User danishan
    IdentityFile ~/.ssh/id_ed25519
    Port 2222
```

---

### b) Enable 2FA (Optional)

```bash
sudo apt install libpam-google-authenticator
google-authenticator
```

* Adds TOTP-based 2FA for SSH logins.

---

## **Outcome**

* VPS is **secured with key-based SSH**.
* Root login is disabled; brute-force mitigated.
* Local machine can connect **easily via IP or domain** using `ssh-config` aliases.
* Optional: Custom SSH port and 2FA for higher security.
