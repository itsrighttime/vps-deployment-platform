
# **itsRIGHTtime Custom MOTD Setup**

## **Objective**

Display a **dynamic, branded login banner** for Ubuntu servers that includes:

* User greeting
* System info (IP, uptime, memory, disk)
* Last login info
* Domain and admin contact
* Unauthorized access warning
* Colored, readable format
* Persistent across SSH and local logins

---

## **1. Create the Custom MOTD Script**

Create a new file:

```bash
sudo nano /etc/profile.d/custom-login.sh
```

Paste the following script:

```bash
#!/bin/bash
# ------------------- Info Variables -------------------
USER=$(whoami)
HOST=$(hostname)
IP=$(hostname -I | awk '{print $1}')
UPTIME=$(uptime -p)
LOGIN_TIME=$(date)
MEMORY=$(free -h | awk '/Mem:/ {print $3 " / " $2}')
DISK=$(df -h / | awk 'NR==2 {print $5 " of " $2}')
LAST_LOGIN=$(last -i -n 2 "$USER" | awk 'NR==2 {print ""$4, $5, $6, $7, $8, "from", $3}')
DOMAIN=$(hostname -f 2>/dev/null || hostname)
ADMIN="danishan@${DOMAIN}"

# ------------------- Colors -------------------
BOLD='\e[1m'
RESET='\e[0m'
BLUE='\e[34m'
LIGHT_BLUE='\e[96m'
CYAN='\e[36m'
GREEN='\e[32m'
YELLOW='\e[33m'
MAGENTA='\e[35m'
RED='\e[31m'

# ------------------- Message -------------------
echo -e ""
echo -e "${CYAN}----------------------- itsRIGHTtime -----------------------${RESET}"
echo -e ""
echo -e " ${BOLD}Welcome, ${USER}!${RESET}"
echo -e " Hostname     : ${GREEN}$HOST${RESET}"
echo -e " IP Address   : ${BLUE}$IP${RESET}"
echo -e " Login Time   : ${YELLOW}$LOGIN_TIME${RESET}"
echo -e " Uptime       : ${YELLOW}$UPTIME${RESET}"
echo -e " Last Login   :  $LAST_LOGIN"
echo ""
echo -e " Memory Used  : ${MAGENTA}$MEMORY${RESET}"
echo -e " Disk Used    : ${MAGENTA}$DISK${RESET}"
echo ""
echo -e " Domain       : ${LIGHT_BLUE}${DOMAIN}${RESET}"
echo -e " Admin        : ${LIGHT_BLUE}${ADMIN}${RESET}"
echo ""
echo -e " ${RED}${BOLD}Unauthorized access is prohibited and logged.${RESET}"
echo -e "${CYAN}------------------------------------------------------------${RESET}"
```

---

## **2. Set Permissions**

Make the script executable:

```bash
sudo chmod +x /etc/profile.d/custom-login.sh
```

**Why:** Only executable scripts in `/etc/profile.d/` are run at login.

---

## **3. Disable Default Ubuntu MOTD**

Ubuntu runs scripts in `/etc/update-motd.d/` and `/etc/motd`. To prevent conflicts:

```bash
sudo chmod -x /etc/update-motd.d/*
sudo rm -f /etc/motd
sudo touch /etc/motd
```

* Disables default dynamic MOTD scripts.
* Ensures `/etc/motd` is empty and does not override your script.

---

## **4. Disable Landscape & Pro Messages**

Optional, to prevent update/service notifications:

```bash
sudo systemctl disable landscape-client
sudo pro config set apt_news=false
```

---

## **5. Optional: Disable PAM MOTD**

If default login messages still appear:

```bash
sudo nano /etc/pam.d/sshd
```

Comment out lines like:

```text
# session    optional     pam_motd.so motd=/run/motd.dynamic
# session    optional     pam_motd.so noupdate
```

Do the same in `/etc/pam.d/login` if needed.

---

## **6. Test the MOTD**

1. Open a new SSH session:

```bash
ssh danishan@itsrighttime.group
```

or using the VPS IP:

```bash
ssh danishan@72.60.201.63
```

2. You should see a **colorful, branded welcome banner** with all system info.

---

## **7. Future Enhancements (Optional)**

* Fetch **public IP** via `curl ifconfig.me`.
* Show **SSH login attempts** summary from `/var/log/auth.log`.
* Add **random quotes or motivational messages**.
* Include **running services or failed units** using `systemctl --failed`.

---

## **Outcome**

* Every login (SSH or local) now shows a **branded, dynamic MOTD**.
* Default Ubuntu messages and landscape notifications are disabled.
* Unauthorized access warnings are clearly visible.

