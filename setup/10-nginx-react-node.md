# **itsRIGHTtime VPS NGINX Setup for React.js Frontend + Express.js Backend**

## **Objective**

* Serve **React frontend** via NGINX as static files.
* Reverse proxy **Express.js backend API** on the same domain.
* Enable HTTPS with Let’s Encrypt.
* Optimize for security and performance.

---

## **1. Build React App**

On your local machine or CI/CD pipeline:

```bash
cd /path/to/react-app
pnpm install
pnpm run build
```

* This generates the production-ready static files in `/build`.

Copy build folder to VPS, for example:

```bash
scp -r build/ danishan@itsrighttime.group:/opt/irt-dev/projects/frontend-engineering/react-app/
```

---

## **2. Configure Express.js Backend**

* Place backend code under `/opt/irt-dev/projects/backend-infrastructure/custom-backend/express-app/`
* Ensure app listens **only on localhost**, e.g., port 5000:

```javascript
const PORT = process.env.PORT || 5000;
app.listen(PORT, '127.0.0.1', () => console.log(`Backend running on ${PORT}`));
```

* Use **PM2** to run Express:

```bash
cd /opt/irt-dev/projects/backend-infrastructure/custom-backend/express-app/
pm2 start index.js --name express-api
pm2 save
```

---

## **3. Configure NGINX Server Block**

Create NGINX configuration:

```bash
sudo nano /etc/nginx/sites-available/itsrighttime.group
```

Example configuration:

```nginx
server {
    listen 80;
    server_name itsrighttime.group www.itsrighttime.group;

    root /opt/irt-dev/projects/frontend-engineering/react-app/build;
    index index.html;

    # Serve React frontend
    location / {
        try_files $uri /index.html;
    }

    # Proxy API requests to Express backend
    location /api/ {
        proxy_pass http://127.0.0.1:5000/;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }

    # Optional: health check endpoint
    location /health {
        proxy_pass http://127.0.0.1:5000/health;
    }
}
```

Enable the site:

```bash
sudo ln -s /etc/nginx/sites-available/itsrighttime.group /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```

---

## **4. Enable SSL with Let’s Encrypt**

```bash
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d itsrighttime.group -d www.itsrighttime.group
```

* Follow prompts to enable HTTPS and redirect HTTP → HTTPS.

Verify renewal:

```bash
sudo certbot renew --dry-run
```

---

## **5. Firewall Configuration**

```bash
sudo ufw allow 'Nginx Full'
sudo ufw status
```

* Ensures ports **80** and **443** are open.
* SSH port must already be allowed.

---

## **6. Test Deployment**

1. Open browser: `https://itsrighttime.group` → React frontend loads.
2. Test API endpoints: `https://itsrighttime.group/api/test` → Routed to Express.js backend.
3. Check NGINX logs:

```bash
sudo tail -f /var/log/nginx/access.log
sudo tail -f /var/log/nginx/error.log
```

---

## **7. Optional Optimizations**

* Enable **gzip** for frontend files in `/etc/nginx/nginx.conf`:

```nginx
gzip on;
gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;
```

* **HTTP/2** is enabled automatically with Certbot SSL.
* Enable **caching headers** for React static files:

```nginx
location / {
    root /opt/irt-dev/projects/frontend-engineering/react-app/build;
    index index.html;
    try_files $uri /index.html;
    add_header Cache-Control "public, max-age=31536000, immutable";
}
```

* For frequent API requests, add **rate limiting** in NGINX.

---

## **Outcome**

* React frontend served efficiently via NGINX.
* Express backend runs on PM2 and is reverse-proxied through NGINX.
* HTTPS enabled with auto-renewing SSL.
* Single domain handles frontend and API requests securely.
* Production-ready deployment on VPS with logging, firewall, and monitoring options.
