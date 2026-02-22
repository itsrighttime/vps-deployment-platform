
# **itsRIGHTtime VPS MySQL Setup & User Management**

## **Objective**

* Install MySQL securely.
* Configure root password and authentication plugin.
* Create application users with proper privileges.
* Optionally enable remote access if needed.

---

## **1. Install MySQL Server**

```bash
sudo apt update
sudo apt install mysql-server -y
```

Verify MySQL service:

```bash
sudo systemctl status mysql
```

---

## **2. Secure MySQL Installation**

Run the built-in security script:

```bash
sudo mysql_secure_installation
```

Follow prompts:

* Set **root password** (if not already).
* Remove **anonymous users**.
* Disallow **root login remotely** (recommended).
* Remove **test database**.
* Reload privilege tables.

**Why:** Ensures MySQL is not open to default or anonymous access.

---

## **3. Configure Root Authentication**

Login to MySQL:

```bash
sudo mysql -u root -p
```

Set root to use **mysql_native_password** for compatibility:

```sql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'YourRootPassword';
FLUSH PRIVILEGES;
```

* Replace `YourRootPassword` with a strong password.
* Ensures traditional password-based login for scripts and admin tools.

---

## **4. Create Application User(s)**

For your DEV apps, create a dedicated user:

```sql
CREATE USER 'danishan'@'localhost' IDENTIFIED BY 'YourSecurePassword';
GRANT ALL PRIVILEGES ON *.* TO 'danishan'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

* `localhost`: Restricts login to local server (more secure).

For remote access (if required):

```sql
CREATE USER 'danishan'@'%' IDENTIFIED BY 'Password';
GRANT ALL PRIVILEGES ON *.* TO 'danishan'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

* `%` allows connections from any IP.
* **Important:** Only open MySQL port in firewall if remote access is required.

---

## **5. Test User Login**

Local:

```bash
mysql -u danishan -p
```

Remote:

```bash
mysql -u danishan -h itsrighttime.group -p
```

* Replace `itsrighttime.group` with server IP/domain.

---

## **6. Optional Security Enhancements**

### a) Bind MySQL to Localhost Only

Edit `/etc/mysql/mysql.conf.d/mysqld.cnf`:

```ini
bind-address = 127.0.0.1
```

* Ensures MySQL is **not exposed publicly**.

Restart MySQL:

```bash
sudo systemctl restart mysql
```

### b) Firewall Rules for Remote MySQL

If remote access is required:

```bash
sudo ufw allow 3306/tcp
```

* Otherwise, keep port blocked for security.

### c) Enable Automatic Updates

```bash
sudo apt install unattended-upgrades -y
sudo dpkg-reconfigure --priority=low unattended-upgrades
```

* Keeps MySQL security patches up to date.

---

## **7. Backup Strategy**

* Daily / Weekly backups to `/shared/backups/mysql/`.
* Use `mysqldump`:

```bash
mysqldump -u danishan -p --all-databases > /shared/backups/mysql/all_databases_$(date +%F).sql
```

* Automate via cron:

```bash
crontab -e
# Example: Backup at 2 AM daily
0 2 * * * mysqldump -u danishan -pYourSecurePassword --all-databases > /shared/backups/mysql/all_databases_$(date +\%F).sql
```

---

## **Outcome**

* MySQL installed and secured.
* Root and application users configured with strong passwords.
* Remote access is optional and secured by firewall rules.
* Backup strategy ensures data safety.
* Compatible with Node.js apps deployed via PM2.


