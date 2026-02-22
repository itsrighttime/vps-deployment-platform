# **1. DEV Projects**

### **Project: itsrighttime server**

| Property        | Value                                                                  |
| --------------- | ---------------------------------------------------------------------- |
| Category        | Website                                                                |
| Type            | Node.js                                                                |
| Domain / URL    | itsrighttime.group                                                     |
| Port(s)         | 5000                                                                   |
| Process Manager | PM2                                                                    |
| Directory Path  | /opt/irt-dev/projects/web-development/itsrighttime/itsrighttime_server |
| Source Code     | itsrighttime/itsrighttime_server.git                                   |
| PM2 Service Name  |  irt-backend                                                         |
| PM2 Config      | /opt/irt-dev/projects/web-development/ecosystem.config.js              |
| Log Path        | ~/.pm2/logs                                                            |
| Notes           | Backend server; API endpoints for frontend                             |


### **Project: itsrighttime UI**

| Property        | Value                                                                                          |
| --------------- | ---------------------------------------------------------------------------------------------- |
| Category        | Website                                                                                        |
| Type            | React                                                                                          |
| Domain / URL    | itsrighttime.group                                                                             |
| Port(s)         | 80/443                                                                                         |
| Process Manager | NGINX                                                                                          |
| Directory Path  | /opt/irt-dev/projects/web-development/itsrighttime/itsrighttime_ui                             |
| Soruce code     | itsrighttime/itsrighttime_ui.git                                                               |
| Log Path        | access: /var/log/nginx/itsrighttime_access.log<br>error: /var/log/nginx/itsrighttime_error.log |
| NGINX Config    | /etc/nginx/sites-available/itsrighttime.conf                                                   |
| Notes           | Frontend served via NGINX; proxies API to backend                                              |


### **Project: itsrighttime DEV Web server**

| Property        | Value                                                                  |
| --------------- | ---------------------------------------------------------------------- |
| Category        | Website                                                                |
| Type            | Node.js                                                                |
| Domain / URL    | dev.itsrighttime.group                                                 |
| Port(s)         | 5002                                                                   |
| Process Manager | PM2                                                                    |
| Directory Path  | /opt/irt-dev/projects/web-development/irt-dev-web/dev-server           |
| Source Code     | itsrighttime/dev-server.git                                            |
| PM2 Service Name  |  irt-dev-backend                                                     |
| PM2 Config      | /opt/irt-dev/projects/web-development/ecosystem.config.js              |
| Log Path        | ~/.pm2/logs                                                            |
| Notes           | Backend server; API endpoints for frontend                             |


### **Project: itsrighttime DEV Web UI**

| Property        | Value                                                                                          |
| --------------- | ---------------------------------------------------------------------------------------------- |
| Category        | Website                                                                                        |
| Type            | React                                                                                          |
| Domain / URL    | dev.itsrighttime.group                                                                         |
| Port(s)         | 80/443                                                                                         |
| Process Manager | NGINX                                                                                          |
| Directory Path  | /opt/irt-dev/projects/web-development/irt-dev-web/dev-ui                                       |
| Soruce code     | itsrighttime/dev-ui.git                                                                        |
| Log Path        | access: /var/log/nginx/itsrighttime_access.log<br>error: /var/log/nginx/itsrighttime_error.log |
| NGINX Config    | /etc/nginx/sites-available/dev.itsrighttime.group                                              |
| Notes           | Frontend served via NGINX; proxies API to backend                                              |


### **Project: itsrighttime CREATIVE Web server**

| Property        | Value                                                                  |
| --------------- | ---------------------------------------------------------------------- |
| Category        | Website                                                                |
| Type            | Node.js                                                                |
| Domain / URL    | cre.itsrighttime.group                                                 |
| Port(s)         | 5001                                                                   |
| Process Manager | PM2                                                                    |
| Directory Path  | /opt/irt-dev/projects/web-development/irt-cre-web/creative_server      |
| Source Code     | itsrighttime/dev-server.git                                            |
| PM2 Service Name  |  irt-cre-backend                                                     |
| PM2 Config      | /opt/irt-dev/projects/web-development/ecosystem.config.js              |
| Log Path        | ~/.pm2/logs                                                            |
| Notes           | Backend server; API endpoints for frontend                             |


### **Project: itsrighttime CREATIVE Web UI**

| Property        | Value                                                                                          |
| --------------- | ---------------------------------------------------------------------------------------------- |
| Category        | Website                                                                                        |
| Type            | React                                                                                          |
| Domain / URL    | cre.itsrighttime.group                                                                         |
| Port(s)         | 80/443                                                                                         |
| Process Manager | NGINX                                                                                          |
| Directory Path  | /opt/irt-dev/projects/web-development/irt-cre-web/creative_ui                                  |
| Soruce code     | itsrighttime/creative_ui.git                                                                   |
| Log Path        | access: /var/log/nginx/itsrighttime_access.log<br>error: /var/log/nginx/itsrighttime_error.log |
| NGINX Config    | /etc/nginx/sites-available/cre.itsrighttime.group                                              |
| Notes           | Frontend served via NGINX; proxies API to backend                                              |

### **Project: Protfolio danishn4u.in (UI)**

| Property        | Value                                                                                          |
| --------------- | ---------------------------------------------------------------------------------------------- |
| Category        | Website                                                                                        |
| Type            | React                                                                                          |
| Domain / URL    | danishn4u.in                                                                                   |
| Port(s)         | 80/443 -> 3008                                                                                 |
| Process Manager | NGINX                                                                                          |
| Directory Path  | /opt/irt-dev/projects/web-development/danishan4u/seo-ui                                        |
| Soruce code     | https://github.com/Danishan1/danishan4u.git                                                    |
| Start Script    | pm2 start pnpm --name danishan4u-frontend -- start                                             |
| PM2 Service Name  |  danishan4u-frontend                                                                         |
| Log Path        | access: /var/log/nginx/itsrighttime_access.log<br>error: /var/log/nginx/itsrighttime_error.log |
| NGINX Config    | /etc/nginx/sites-available/danishan4u.in                                                       |
| Notes           | Frontend served via NGINX; proxies API to backend                                              |

### **Project: Protfolio danishn4u.in (Server)**

| Property        | Value                                                                                          |
| --------------- | ---------------------------------------------------------------------------------------------- |
| Category        | Website                                                                                        |
| Type            | React                                                                                          |
| Domain / URL    | danishn4u.in                                                                                   |
| Port(s)         | 80/443 -> 5008                                                                                 |
| Process Manager | NGINX                                                                                          |
| Directory Path  | /opt/irt-dev/projects/web-development/danishan4u/server                                        |
| Soruce code     | https://github.com/Danishan1/danishan4u.git                                                    |
| Start Script    | pm2 start src/server.js --name danishan4u-backend                                              |
| PM2 Service Name  |  danishan4u-backend                                                                          |
| PM2 Config      | /opt/irt-dev/projects/web-development/ecosystem.config.js                                      |
| Log Path        | ~/.pm2/logs                                                                                    |
| Notes           | Backend server; API endpoints for frontend                                                     |




# **2. Testing Environment (Pre-Deployment)**

### **Project: Example-Test**

| Property        | Value                                                        |
| --------------- | ------------------------------------------------------------ |
| Category        | Website / QA                                                 |
| Type            | Node.js + React                                              |
| Domain / URL    | test.example.com                                             |
| Port(s)         | 5001 / 443                                                   |
| Process Manager | PM2 / NGINX                                                  |
| Directory Path  | /opt/test/projects/example-test                              |
| Log Path        | PM2: ~/.pm2/logs<br>NGINX: /var/log/nginx/example-test_*.log |
| Notes           | Pre-production / QA environment; mirrors production setup    |


# **3. WorkSpace Products**

### **Project: sample-project**

| Property        | Value                   |
| --------------- | ----------------------- |
| Category        | Example                 |
| Type            | Node.js (MERN)          |
| Domain / URL    | example.com             |
| Port(s)         | 3000                    |
| Process Manager | PM2                     |
| Directory Path  | /opt/dev/sample-project |
| Log Path        | /shared/logs/sample.log |
| Notes           | Main product server     |


### **Project: sample-admin**

| Property        | Value                             |
| --------------- | --------------------------------- |
| Category        | Example                           |
| Type            | React                             |
| Domain / URL    | admin.example.com                 |
| Port(s)         | 443                               |
| Process Manager | NGINX                             |
| Directory Path  | /opt/dev/sample-admin             |
| Log Path        | /var/log/nginx/sample-admin_*.log |
| Notes           | Admin panel frontend              |


