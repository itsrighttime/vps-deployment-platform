
# **itsRIGHTtime Server Information Template**

## Server Overview

- **Server Name / Hostname:** itsrighttime.group
- **IP Address / Domain:** 72.60.201.63 / itsrighttime.group
- **Environment:** Production
- **OS & Version:** Ubuntu 22.04 LTS
- **Date Deployed:** 28-09-2025
- **Owner / Responsible Employee(s):** Danishan


## 1. Hardware & Resources

* **CPU:** 4 core
* **RAM:** 16
* **Disk Storage:** 200 GB
* **Disk Usage:** [Enter current usage]
* **Network Bandwidth / Limits:** [Enter limits if any]


## 2. Access & Accounts

**SSH Access:**

* Admin user(s): `danishan`
* DEV users: No
* WorkSpace users: No
* Notes on key-based authentication / restrictions:

  * Root login disabled (`PermitRootLogin no`)
  * Password login disabled (`PasswordAuthentication no`)
  * SSH key authentication only
  * Custom SSH port (optional, if implemented)

**Sudo Access:**

* `danishan`

**Password Policies / Key Rotation:**

* MySQL root and user passwords set
* SSH keys generated for all admins
* Key rotation recommended every 6–12 months


## 3. Installed Services & Packages

| Service / Software       | Version                   | Purpose                       | Notes                                                                   |
| ------------------------ | ------------------------- | ----------------------------- | ----------------------------------------------------------------------- |
| NGINX                    | [Check `nginx -v`]        | Web server / Reverse proxy    | Configured for static pages and Node.js apps, HTTPS enabled via Certbot |
| MySQL Server             | [Check `mysql --version`] | Database                      | Root and app users configured, remote access enabled                    |
| Node.js                  | v22                       | Runtime for DEV               | Global tools installed: PNPM, PM2                                       |
| PM2                      | Latest                    | Node.js process management    | Auto-start enabled                                                      |
| Git                      | Latest                    | Version control               | SSH key-based access configured                                         |
| Fail2Ban                 | Latest                    | Intrusion prevention          | Protects SSH from brute-force attacks                                   |
| UFW                      | Latest                    | Firewall                      | Ports 80, 443 open                                                      |
| htop, iotop, iftop, ncdu | Latest                    | Monitoring / System utilities | For real-time system diagnostics                                        |
| Unattended Upgrades      | Latest                    | Automatic security updates    | Enabled                                                                 |
| curl, wget, unzip        | Latest                    | Utility tools                 | Installed for server management                                         |


## 4. Folder Structure

```
/opt/irt-dev/
├─ projects/
│  ├─ web-development/
│  ├─ mobile-application-development/
│  ├─ backend-infrastructure/
│  ├─ frontend-engineering/
│  ├─ mvp-startup-acceleration/
│  ├─ maintenance-support/
│  └─ emerging-technology/
├─ scripts/
└─ docs/

/opt/irt-ws/products/
├─ letsSecure/
├─ letsIAM/
├─ letsDiscuss/
├─ letsNotify/
├─ letsNoteThis/
├─ letsWrite/
├─ letsSchedule/
└─ upcoming_products/
   ├─ letsCreate/
   ├─ letsCollaborate/
   └─ etc.

/shared/
├─ backups/
├─ scripts/
├─ secrets/
├─ logs/
└─ docs/
```

* Each product folder contains: `code/`, `config/`, `logs/`, `docs/`


## 11. Maintenance Schedule

| Task                | Frequency | Responsible | Notes                                    |
| ------------------- | --------- | ----------- | ---------------------------------------- |
| OS Updates          | Weekly    | Danishan    | `sudo apt update && sudo apt upgrade -y` |
| Backup Verification | Daily     | Danishan    | Check backup integrity                   |
| Log Cleanup         | Monthly   | Danishan    | Rotate old logs                          |
| Security Patches    | As needed | Danishan    | Apply critical updates immediately       |
| NGINX / Node Health | Daily     | Danishan    | Verify services running                  |


## 12. Notes & Additional Information

* SSH key-based login enforced; root login disabled.
* Custom MOTD configured for login awareness.
* Swap file: 5GB added for memory optimization.
* Node.js apps managed via PM2, served through NGINX reverse proxy.
* HTTPS enforced with Let’s Encrypt certificates.
* Recommended: Consider 2FA for SSH, regular security audits, and automated monitoring alerts.

