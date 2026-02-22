# itsRIGHTtime VPS Deployment & Management

## **Overview**

This document outlines the **setup, configuration, and management** of the Ubuntu VPS hosting the itsRIGHTtime Tech Ecosystem. It covers:

- Firewall security (UFW)
- Version control (Git & GitHub)
- Node.js environment with PNPM and PM2
- Database management (MySQL)
- System utilities, swap, and timezone
- NGINX reverse proxy for React + Express apps
- VPS directory structure, backups, logs, and maintenance

The guide ensures **production-ready, secure, and scalable deployment practices**.

## **1. Firewall Setup (UFW)**

**Purpose:** Protect server by allowing only essential traffic.

- Default policies: `deny incoming`, `allow outgoing`
- Allowed services: SSH, HTTP, HTTPS, optional app ports
- Security enhancements: SSH rate limiting, logging
- Verification: `sudo ufw status numbered`

**Outcome:** Server is protected; only explicitly allowed ports accessible.

## **2. Git & GitHub Integration**

**Purpose:** Secure version control and deployment via SSH.

- Install Git and configure global identity
- Generate VPS SSH key, add to GitHub
- Configure local SSH for VPS and GitHub
- Clone repositories under `/opt/irt-dev/projects/`

**Outcome:** Enables secure, production-ready Git workflow for DEV projects.

## **3. Node.js, PNPM & PM2 Setup**

**Purpose:** Modern JavaScript runtime for backend apps.

- Install Node.js 22 LTS, PNPM, PM2
- Initialize sample Node.js projects
- Run apps as background services with PM2
- Enable auto-start on server reboot
- Monitor logs and performance

**Outcome:** Node.js apps deployed reliably, auto-restarted, and monitored.

## **4. MySQL Setup & User Management**

**Purpose:** Install, secure, and manage database access.

- Secure installation via `mysql_secure_installation`
- Create root and application users with least privilege
- Optional remote access via firewall
- Backup strategy using `mysqldump` with cron automation

**Outcome:** Database ready for production apps with strong access control and backup strategy.

## **5. Swap, Timezone & Essential Utilities**

**Purpose:** System stability, correct time, and monitoring tools.

- Set timezone (e.g., `Asia/Kolkata`)
- Configure swap space (e.g., 5GB) for memory management
- Install utilities: `htop`, `iotop`, `iftop`, `ncdu`, `curl`, `wget`, `git`, `unzip`
- Enable unattended security updates

**Outcome:** Server stable, monitored, and protected with automated updates.

## **6. NGINX Reverse Proxy (React + Express)**

**Purpose:** Serve frontend and backend securely via single domain.

- Build React frontend → copy `/build` folder to VPS
- Express backend listens on localhost (e.g., port 5000)
- NGINX serves React and proxies `/api/` to Express
- Enable HTTPS with Let’s Encrypt
- Firewall allows NGINX full (ports 80 & 443)

**Outcome:** Production-ready deployment, HTTPS-enabled, single-domain access, logging enabled.

## **7. Server Overview & Documentation**

### **Server Specs**

| Item         | Value              |
| ------------ | ------------------ |
| Hostname     | itsrighttime.group |
| IP Address   | 72.60.201.63       |
| OS           | Ubuntu             |
| CPU          | 4 core             |
| RAM          | 16 GB              |
| Disk         | 200 GB             |
| VPS Provider | Hostinger          |

### **Databases & Users**

- Multiple DBs with **least privilege principle**
- Backup schedule: daily, weekly, monthly
- Sample user roles: `proj1_write`, `proj1_admin`, `analytics_ro`

### **Linux Users & Groups**

- Users: `root`, `danishan`, `support`, `hello`, `no-reply`, `dev`, `creative`, `workspace`
- Groups: `root`, `sudo`, `devs`
- SSH key-based access only

### **Backups & Scripts**

- `/shared/backups/` → databases, projects, products
- `/shared/scripts/` → automation, monitoring, deployment
- `/opt/irt-dev/scripts/` → DEV-specific deployment scripts
- `/opt/irt-ws/scripts/` → WorkSpace product scripts

### **Secrets & Logs**

- `/shared/secrets/` → environment variables, API keys (restricted access)
- `/shared/logs/` → central log aggregation for monitoring

### **Firewall & Services**

| Service        | Port(s)                        | Notes                        |
| -------------- | ------------------------------ | ---------------------------- |
| SSH            | 22                             | Fail2Ban optional            |
| Nginx          | 80/443                         | React frontend + Express API |
| MySQL          | 3306                           | localhost only               |
| SMTP/IMAP/POP3 | 25,587,465 / 143,993 / 110,995 | Mail services                |

- Services managed via systemd or PM2 (`pm2 list` for Node apps)

## **8. VPS Directory Structure**

```
/opt/irt-dev/
├─ projects/
│  ├─ web-development/
│  ├─ backend-infrastructure/
│  └─ ...
├─ scripts/
└─ docs/

/opt/irt-ws/products/
├─ letsSecure/
├─ letsIAM/
└─ upcoming_products/

/shared/
├─ backups/
├─ scripts/
├─ secrets/
├─ logs/
└─ docs/
```

**Notes:**

- Each project/product has `code/`, `config/`, `logs/`, `docs/`
- `/shared` contains central resources
- Enforces modular, flat, and scalable structure

## **9. Maintenance & Monitoring**

- Health checks via cron and scripts (`health_check.sh`)
- Regular system updates: `sudo apt update && sudo apt upgrade -y`
- Backup verification and logging
- PM2 monitoring for Node.js apps
- NGINX logs for frontend/backend requests
- Centralized alerts (email/Slack optional)

## **10. Key Benefits**

- Secure, production-ready VPS environment
- Standardized folder structure for DEV and WorkSpace projects
- Modular and scalable design
- Centralized monitoring, logging, and backups
- Facilitates collaboration and smooth onboarding

This README.md now serves as a **comprehensive VPS operations manual**, covering **everything from system hardening, version control, runtime environment, database, frontend/backend deployment, to server monitoring and backups**.

It’s ready to be **used internally as the master reference for all itsRIGHTtime VPS setups**.
