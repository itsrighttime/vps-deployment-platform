# Server Documentation & Maintenance Log


##  Server Overview

| Item              | Value / Details        |
| ----------------- | ---------------------- |
| Hostname          | itsrighttime.group     |
| IP Address(es)    | 72.60.201.63           |
| OS / Distro       | Ubuntu                 |
| Kernel Version    |                        |
| CPU               | 4 core                 |
| RAM               | 16 GB                  |
| Disk(s)           | 200 GB                 |
| VPS Provider      | Hostinger              |
| SSH Users Allowed |                        |
| Firewall Setup    | UFW                    |
| Last Updated      | 29-09-2025             |


## Databases

| DB Type  | Version | Host/Path          | Port | Users & Roles | Backup Enabled | Notes |
| -------- | ------- | ------------------ | ---- | ------------- | -------------- | ----- |
| MySQL    |         | localhost:/var/lib | 3306 | root, dev     | Yes/No         |       |


## MySQL Users & Permissions

| Username     | Host   | Database       | Role / Access Type | Permissions Granted                 | Notes                      |
| ------------ | ------ | -------------- | ------------------ | ----------------------------------- | -------------------------- |
| itsrighttime | %      | itsrighttime   | Read/Write         | ALL                                 | Used by itsrighttime website management |
| proj1_write  | %      | project1_db    | Read/Write         | SELECT, INSERT, UPDATE, DELETE      | Used by app backend        |
| proj1_admin  | %      | project1_db    | Admin (limited)    | ALL on project1_db                  | For dev team only          |
| analytics_ro | %      | analytics_db   | Reporting          | SELECT                              | Power BI connector         |
| iam_writer   | %      | iam_db         | Service Account    | INSERT, UPDATE, SELECT              | IAM product only           |

*Best Practices:*
- Each project has **separate DB users**.
- Use **least privilege principle** (no global root usage).
- Service accounts mapped **1:1 with apps** (not shared).


## Linux Users

| Username  | UID | Groups        | Home Directory | Shell | Notes                |
| --------- | --- | ------------- | -------------- | ----- | -------------------- |
| root      | 0   | root          | /root          | bash  | Superuser            |
| danishan  | 1000| sudo, devs    | /home/danishan | bash  | Primary admin        |


## Linux Users fro Mail purpose
- danishan
- support
- no-reply
- hello
- dev
- creative
- workspace



## Linux Groups

| Group Name | GID  | Members                     |
| ---------- | ---- | --------------------------- |
| root       | 0    | root                        |
| sudo       | 27   | root, danishan              |
| devs       | 1001 | danishan, teammate1, etc.   |


## Backups

* **Backup Location:** `/shared/backups/`
* **Frequency:** Daily / Weekly / Monthly
* **Backup Retention Policy:** [Specify retention rules]
* **Responsible Person:** Danishan
* **Verification Process:** [How to test backups]


## Scripts & Automation

| Script            | Path                                   | Purpose                            | Owner    | Notes              |
| ----------------- | -------------------------------------- | ---------------------------------- | -------- | ------------------ |
| deploy_project.sh | /opt/irt-dev/scripts/deploy_project.sh | Deploy DEV projects                | Danishan | Use after Git pull |
| health_check.sh   | /shared/scripts/health_check.sh        | Server & service health monitoring | Danishan | Cron every 30 min  |


## Secrets & Environment Variables

* **Location:** `/shared/secrets/`
* **Access:** Restricted to `danishan`
* **Notes:** Contains DB passwords, API keys, private SSH keys
* **Handling:** Never commit to Git / Version control


## Logs & Monitoring

* **Central log folder:** `/shared/logs/`
* **Monitoring setup:** htop, iotop, iftop, Netdata (optional), Fail2Ban logs
* **Log rotation schedule:** Monthly / Configured via `logrotate`
* **Alerting:** Email / Slack integration (optional)


## **Network & Firewall (UFW Rules)**

| Service          | Port(s)      | Protocol | Allowed IPs    | Notes / Description                          |
| ---------------- | ------------ | -------- | -------------- | -------------------------------------------- |
| SSH              | 22           | TCP      | Any            | Fail2Ban enabled; consider restricting by IP |
| Web (HTTP)       | 80           | TCP      | Any            | Nginx; automatic redirect to HTTPS           |
| Web (HTTPS)      | 443          | TCP      | Any            | Nginx + Certbot SSL                          |
| Nginx Full       | 80, 443      | TCP      | Any            | Includes both HTTP & HTTPS                   |
| SMTP (Mail)      | 25, 587, 465 | TCP      | Any            | Mail sending/relay ports                     |
| IMAP (Mail)      | 143, 993     | TCP      | Any            | IMAP email retrieval; 993 = secure SSL/TLS   |
| POP3 (Mail)      | 110, 995     | TCP      | Any            | POP3 email retrieval; 995 = secure SSL/TLS   |
| MySQL / Database | 3306         | TCP      | localhost only | No external access                           |

 


## Services & Daemons

| Service   | Manager  | Status | Autostart | Notes |
| --------- | -------- | ------ | --------- | ----- |
| nginx     | systemd  | active | enabled   |       |
| mysql     | systemd  | active | enabled   |       |
| pm2       | user:dan | active | enabled   | Node apps |


## Maintenance Log

| Date       | Task Performed                     | Performed By | Notes                  |
| ---------- | ---------------------------------- | ------------ | ---------------------- |
| 2025-09-30 | Installed MySQL + Secured          | Danishan     | Applied root password  |
| 2025-10-01 | Setup Nginx reverse proxy for app1 | Danishan     | Linked domain to /opt  |

