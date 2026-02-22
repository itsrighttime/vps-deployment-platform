# **itsRIGHTtime VPS: Complete Deployment & Hardening Checklist**

**Server Name / Hostname:** itsrighttime.group
**IP Address / Domain:** 72.60.201.63 / itsrighttime.group
**Environment:** Production
**OS & Version:** Ubuntu 22.04 LTS
**Owner / Responsible Employee(s):** danishan

---

## **1. Initial Server Setup**

1. **System Update & Upgrade**

```bash
sudo apt update && sudo apt upgrade -y
```

2. **Create Admin User**

```bash
sudo adduser danishan
sudo usermod -aG sudo danishan
```

3. **SSH Hardening**

* Generate SSH key pair locally:

```bash
ssh-keygen -t ed25519 -C "danishan089@gmail.com"
```

* Add public key to VPS:

```bash
mkdir -p ~/.ssh && chmod 700 ~/.ssh
nano ~/.ssh/authorized_keys  # Paste public key
chmod 600 ~/.ssh/authorized_keys
```

* Update `/etc/ssh/sshd_config`:

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

* Restart SSH:

```bash
sudo systemctl restart ssh
```

* **Local SSH config** (`~/.ssh/config`):

```text
Host itsrighttime
  HostName 72.60.201.63
  User danishan
  IdentityFile ~/.ssh/irt_github

Host itsrighttime-domain
  HostName itsrighttime.group
  User danishan
  IdentityFile ~/.ssh/irt_github
```

---

## **2. MOTD / Login Banner**

* Custom login message: `/etc/profile.d/custom-login.sh`
* Executable permissions:

```bash
sudo chmod +x /etc/profile.d/custom-login.sh
sudo chmod -x /etc/update-motd.d/*
sudo rm -f /etc/motd && sudo touch /etc/motd
```

* Disable Ubuntu update/landscape messages:

```bash
sudo pro config set apt_news=false
sudo systemctl disable landscape-client
```

---

## **3. Firewall Setup (UFW)**

```bash
sudo apt install ufw -y
sudo ufw allow ssh
sudo ufw allow http
sudo ufw allow https
sudo ufw enable
sudo ufw status
```

---

## **4. Fail2Ban Installation**

```bash
sudo apt install fail2ban -y
```

* Optional: Customize jail configuration in `/etc/fail2ban/jail.local`.

---

## **5. Swap, Timezone & Utilities**

```bash
sudo fallocate -l 5G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab

sudo timedatectl set-timezone Asia/Kolkata

sudo apt install curl wget git unzip htop iotop iftop ncdu unattended-upgrades -y
sudo dpkg-reconfigure --priority=low unattended-upgrades
```

---

## **6. Node.js, PNPM & PM2**

```bash
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt install -y nodejs
sudo npm install -g pnpm pm2
```

* Backend apps run via PM2:

```bash
pm2 start index.js --name project1-backend
pm2 save
pm2 startup systemd
```

---

## **7. MySQL Setup & User Management**

```bash
sudo apt install mysql-server -y
sudo mysql_secure_installation
```

* Create database and user per project:

```sql
CREATE DATABASE project1_db;
CREATE USER 'project1_user'@'localhost' IDENTIFIED BY 'StrongPassword1';
GRANT ALL PRIVILEGES ON project1_db.* TO 'project1_user'@'localhost';
FLUSH PRIVILEGES;

-- Repeat for project2, project3
```

* Backend `.env` example:

```env
DB_HOST=localhost
DB_USER=project1_user
DB_PASSWORD=StrongPassword1
DB_NAME=project1_db
PORT=5001
NODE_ENV=production
```

---

## **8. NGINX Setup with SSL & Reverse Proxy**

* Install NGINX:

```bash
sudo apt install nginx -y
```

* **Frontend (React)** served via `/build`, backend proxied via `/api`:

```nginx
server {
    listen 80;
    server_name project1.itsrighttime.group;

    root /opt/irt-dev/projects/mern/project1/frontend/build;
    index index.html;

    location / {
        try_files $uri /index.html;
    }

    location /api/ {
        proxy_pass http://127.0.0.1:5001/;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

* Enable sites:

```bash
sudo ln -s /etc/nginx/sites-available/project1.itsrighttime.group /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```

* SSL with Certbot:

```bash
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d project1.itsrighttime.group
sudo certbot --nginx -d project2.itsrighttime.group
sudo certbot --nginx -d project3.itsrighttime.group
```

---

## **9. PM2 Ecosystem for Multiple Projects**

Example `ecosystem.config.js`:

```javascript
module.exports = {
  apps: [
    {
      name: "project1-backend",
      script: "./index.js",
      cwd: "/opt/irt-dev/projects/mern/project1/backend/code",
      env: {
        PORT: 5001,
        NODE_ENV: "production",
        DB_HOST: "localhost",
        DB_USER: "project1_user",
        DB_PASSWORD: "StrongPassword1",
        DB_NAME: "project1_db"
      }
    },
    {
      name: "project2-backend",
      script: "./index.js",
      cwd: "/opt/irt-dev/projects/mern/project2/backend/code",
      env: {
        PORT: 5002,
        NODE_ENV: "production",
        DB_HOST: "localhost",
        DB_USER: "project2_user",
        DB_PASSWORD: "StrongPassword2",
        DB_NAME: "project2_db"
      }
    }
  ]
};
```

* Start all projects:

```bash
pm2 start ecosystem.config.js
pm2 save
pm2 monit
```

---

## **10. Logs & Monitoring**

* PM2 logs: `pm2 logs project1-backend`
* Log rotation via PM2:

```bash
pm2 install pm2-logrotate
pm2 set pm2-logrotate:max_size 10M
pm2 set pm2-logrotate:retain 30
```

* NGINX logs in `/var/log/nginx/`.

---

## **11. Maintenance Schedule**

| Task             | Frequency | Responsible | Notes                                     |
| ---------------- | --------- | ----------- | ----------------------------------------- |
| OS Updates       | Weekly    | Admin       | `sudo apt update && sudo apt upgrade -y`  |
| MySQL Backups    | Daily     | Admin       | Dump databases to `/shared/backups/mysql` |
| Node App Backups | Daily     | Admin       | Include code & .env                       |
| Log Cleanup      | Monthly   | Admin       | Rotate old logs                           |
| SSL Renewal      | Auto      | Certbot     | `sudo certbot renew --dry-run`            |
| Fail2Ban Review  | Weekly    | Admin       | Check banned IPs & logs                   |

---

## **12. Security & Hardening**

* SSH key-based login only.
* Disable root login.
* UFW firewall enabled, only SSH/HTTP/HTTPS open.
* Fail2Ban for brute-force protection.
* MySQL users isolated per project.
* PM2 ensures auto-restart and crash recovery.
* NGINX reverse proxy isolates front/backends per project.

---

## **13. Future Enhancements**

* Add **centralized monitoring**: Netdata, Prometheus, or Grafana.
* CI/CD pipeline for automatic deployment.
* Separate staging and production environments per project.
* Daily automated backups with offsite storage.

