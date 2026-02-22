# **itsRIGHTtime VPS UFW Firewall Setup**

## **Objective**

* Protect the server by allowing only essential traffic.
* Enable firewall rules for HTTP, HTTPS, SSH, and any custom ports.
* Ensure firewall is persistent across reboots.

---

## **1. Install UFW**

```bash
sudo apt update
sudo apt install ufw -y
```

* UFW is lightweight and simplifies iptables management.
* It is pre-installed on Ubuntu in most cases, but this ensures the latest version.

---

## **2. Check UFW Status**

```bash
sudo ufw status verbose
```

* Should show **inactive** initially.

---

## **3. Set Default Policies**

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

**Why:**

* Deny all incoming connections by default.
* Allow all outgoing traffic (server can access external services).

---

## **4. Allow Essential Ports**

### a) SSH

If using default port 22:

```bash
sudo ufw allow 22/tcp
```

If using a custom SSH port (e.g., 2222):

```bash
sudo ufw allow 2222/tcp
```

### b) HTTP & HTTPS (NGINX)

```bash
sudo ufw allow http
sudo ufw allow https
```

* Alternatively, combined rule:

```bash
sudo ufw allow 'Nginx Full'
```

* `Nginx Full` opens **ports 80 and 443**.

---

### c) Optional: Custom Application Ports

```bash
sudo ufw allow 3000/tcp   # Example Node.js app port
sudo ufw allow 3306/tcp   # Example MySQL remote access (if needed)
```

**Tip:** Only open ports you really need; avoid exposing databases publicly.

---

## **5. Enable UFW**

```bash
sudo ufw enable
```

* Confirms activation.
* Firewall rules will persist across reboots.

---

## **6. Verify Rules**

```bash
sudo ufw status numbered
```

* Example output:

```
Status: active

     To                         Action      From
     --                         ------      ----
[ 1] 22/tcp                     ALLOW       Anywhere
[ 2] 80/tcp                     ALLOW       Anywhere
[ 3] 443/tcp                    ALLOW       Anywhere
```

---

## **7. Optional Security Enhancements**

### a) Limit SSH Connections

```bash
sudo ufw limit 22/tcp
```

* Prevents brute-force attacks by limiting connection attempts.

### b) Logging

```bash
sudo ufw logging on
sudo ufw status verbose
```

* Logs firewall activity to `/var/log/ufw.log`.

---

## **Outcome**

* Server is **protected by a firewall**.
* Only SSH, HTTP, HTTPS, and explicitly allowed ports are accessible.
* Default deny policy ensures all unexpected traffic is blocked.
* Logging provides visibility for security audits.

