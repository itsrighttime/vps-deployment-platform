# **itsRIGHTtime VPS Swap, Timezone & Essential Utilities Setup**

## **Objective**

* Configure **swap space** for memory management.
* Set correct **timezone** for logs, cron jobs, and applications.
* Install essential utilities for monitoring, debugging, and system management.

---

## **1. Set Timezone**

Check current timezone:

```bash
timedatectl
```

Set to your desired timezone (Asia/Kolkata):

```bash
sudo timedatectl set-timezone Asia/Kolkata
```

Verify:

```bash
timedatectl
```

**Why:** Ensures all system logs, cron jobs, and applications run with consistent time.

---

## **2. Configure Swap Space**

Check if swap exists:

```bash
swapon --show
```

If none, create a swap file:

```bash
sudo fallocate -l 5G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
```

* `5G` can be adjusted based on your RAM and workload.
* `chmod 600` secures the swap file from unauthorized access.

Make swap persistent across reboots:

```bash
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```

Verify swap is active:

```bash
swapon --show
free -h
```

**Why:**

* Prevents out-of-memory errors.
* Useful for Node.js apps, MySQL, and other services under load.

---

## **3. Install Essential Utilities**

Install monitoring and management tools:

```bash
sudo apt update
sudo apt install -y curl wget git unzip htop iotop iftop ncdu ufw
```

**Tools & Purpose:**

| Utility | Purpose                            |
| ------- | ---------------------------------- |
| `curl`  | Fetch external data, APIs, scripts |
| `wget`  | Download files from the internet   |
| `git`   | Version control                    |
| `unzip` | Extract archives                   |
| `htop`  | Interactive process monitoring     |
| `iotop` | Disk I/O monitoring                |
| `iftop` | Network usage monitoring           |
| `ncdu`  | Disk usage analysis                |
| `ufw`   | Firewall management                |

**Why:** Provides a complete toolset for system monitoring, troubleshooting, and server management.

---

## **4. Enable Unattended Security Updates**

Install and configure automatic updates:

```bash
sudo apt install unattended-upgrades -y
sudo dpkg-reconfigure --priority=low unattended-upgrades
```

* Ensures security patches for OS and packages are applied automatically.
* Reduces risk of vulnerabilities on production server.

---

## **5. Optional System Tweaks**

* **Disable unnecessary services**:

```bash
sudo systemctl disable landscape-client
sudo systemctl disable snapd
```

* **Check disk usage**:

```bash
df -h
```

* **Check memory usage**:

```bash
free -h
```

* **Check swap usage**:

```bash
swapon --show
```

---

## **Outcome**

* Timezone is set for accurate logs and scheduling.
* Swap space is configured, improving system stability.
* Essential monitoring and system utilities are installed.
* Automatic security updates reduce exposure to vulnerabilities.
* Server is ready for production workloads with Node.js, MySQL, and Git deployments.

