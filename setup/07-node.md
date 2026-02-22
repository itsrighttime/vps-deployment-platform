# **itsRIGHTtime VPS Node.js + PNPM + PM2 Setup**

## **Objective**

* Install Node.js for runtime environment.
* Use **PNPM** for package management (faster, disk-efficient).
* Use **PM2** to run Node.js applications as background services with auto-restart, logging, and monitoring.

---

## **1. Install Node.js**

Add NodeSource repository (Node 22 LTS):

```bash
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt install -y nodejs
```

Verify installation:

```bash
node -v
npm -v
```

* `node -v`: Shows Node.js version.
* `npm -v`: Shows NPM version bundled with Node.js.

**Why:** Node.js 22 is LTS, stable, and supports modern features.

---

## **2. Install PNPM**

```bash
sudo npm install -g pnpm
```

Verify:

```bash
pnpm -v
```

**Why PNPM:**

* Faster than npm/yarn
* Deduplicates dependencies across projects
* Efficient disk usage

---

## **3. Install PM2**

```bash
sudo npm install -g pm2
```

Verify:

```bash
pm2 -v
```

**Why PM2:**

* Keeps Node.js apps alive after crashes
* Enables auto-start on server reboot
* Provides monitoring & logs

---

## **4. Initialize a Sample Node.js App**

Create a project folder:

```bash
mkdir -p /opt/irt-dev/projects/backend-infrastructure/custom-backend/nodejs/project1
cd /opt/irt-dev/projects/backend-infrastructure/custom-backend/nodejs/project1
```

Initialize project with PNPM:

```bash
pnpm init
```

Install dependencies, for example Express:

```bash
pnpm add express
```

Create `index.js`:

```javascript
const express = require('express');
const app = express();
const PORT = process.env.PORT || 3000;

app.get('/', (req, res) => {
    res.send('itsRIGHTtime Node.js Server Running!');
});

app.listen(PORT, () => console.log(`Server running on port ${PORT}`));
```

---

## **5. Start App with PM2**

```bash
pm2 start index.js --name project1
```

Check running processes:

```bash
pm2 list
```

* `--name project1`: Assigns friendly name for easier management.

---

## **6. Enable Auto-Start on Server Reboot**

```bash
pm2 startup systemd
```

Follow the command output (it usually gives a systemctl command to run).

Save current PM2 process list:

```bash
pm2 save
```

**Why:** Ensures Node.js apps auto-start on reboot.

---

## **7. View Logs & Monitor App**

```bash
pm2 logs project1
pm2 monit
```

* Logs show runtime messages.
* Monit allows real-time CPU/memory usage monitoring per app.

---

## **8. Deploy Updates via Git + PM2**

1. Pull latest code:

```bash
cd /opt/irt-dev/projects/backend-infrastructure/custom-backend/nodejs/project1
git pull origin main
```

2. Install dependencies (PNPM):

```bash
pnpm install
```

3. Restart app:

```bash
pm2 restart project1
```

**Why:** Smooth updates with zero downtime if using `pm2 reload` for multiple apps.

---

## **9. Optional Enhancements**

* **Environment variables:** Create `.env` file and load with `dotenv`.
* **Cluster mode:**

```bash
pm2 start index.js -i max --name project1
```

* Uses all CPU cores for better performance.

* `max` auto-detects CPU cores.

* **Monitor via web:**

```bash
pm2 web
```

* Access via `http://<server-ip>:9615` to view dashboard.

---

## **Outcome**

* Node.js 22 installed and ready for apps.
* PNPM ensures efficient package management.
* PM2 keeps apps alive, auto-starts on reboot, and monitors performance.
* Deployment via Git and PM2 is simple and reliable.
* Ready to serve multiple DEV projects under `/opt/irt-dev/`.

