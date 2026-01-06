# 🚀 VPS Setup & Node.js Production Environment

**(NVM · PM2 · Git)**

This guide provides a **professional, production-ready approach** to setting up a **Node.js environment** on an **Ubuntu VPS** using **NVM** and **PM2**.

> ❌ Apache2 is **not required**
> ✅ Designed for **Node.js + Nginx + PM2** architecture

---

## 📌 System Requirements

* Ubuntu 20.04 / 22.04 LTS
* Root or sudo access
* Basic Linux terminal knowledge

---

## 🔹 1. System Update & Base Packages

Always keep the system updated before installing anything.

```bash
sudo apt update && sudo apt upgrade -y
```

Install essential utilities:

```bash
sudo apt install -y curl wget git build-essential
```

---

## 🔹 2. Disable Unnecessary Services (Apache2)

Apache2 is **not used** in modern Node.js production deployments.

### Check Apache2 status (if installed)

```bash
sudo systemctl status apache2
```

### Stop & disable Apache2

```bash
sudo systemctl stop apache2
sudo systemctl disable apache2
```

### Optional: Remove Apache2 completely

```bash
sudo apt purge apache2 apache2-utils apache2-bin apache2.2-common -y
sudo apt autoremove -y
```

---

## 🔹 3. Install NVM (Node Version Manager)

NVM allows safe management of multiple Node.js versions and is **recommended for production**.

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash
```

Load NVM into the current shell:

```bash
source ~/.bashrc
```

Verify installation:

```bash
nvm -v
```

---

## 🔹 4. Install Node.js (LTS)

Install the latest **LTS version** of Node.js:

```bash
nvm install --lts
```

Set it as the default version:

```bash
nvm alias default lts/*
```

Verify:

```bash
node -v
npm -v
```

---

## 🔹 5. Install Git

Git is required for version control and deployment.

```bash
sudo apt install git -y
```

Verify:

```bash
git --version
```

---

## 🔹 6. Install PM2 (Process Manager)

PM2 ensures:

* Background execution
* Auto-restart on crash
* Startup on server reboot

```bash
npm install -g pm2
```

Verify installation:

```bash
pm2 -v
```

---

## 🔹 7. Run Node.js Application with PM2

### Start application

```bash
pm2 start app.js --name my-app
```

### View running processes

```bash
pm2 list
```

### Logs

```bash
pm2 logs my-app
```

### Stop / Restart / Delete

```bash
pm2 stop my-app
pm2 restart my-app
pm2 delete my-app
```

---

## 🔹 8. Enable PM2 on Server Reboot

Generate startup script:

```bash
pm2 startup
```

Run the command shown in the output.

Save current process list:

```bash
pm2 save
```

---

## 🔹 9. Common PM2 Utilities

```bash
pm2 logs
pm2 monit
pm2 flush
```

---

## 🔹 10. Recommended Production Stack

| Component       | Purpose             |
| --------------- | ------------------- |
| Node.js (LTS)   | Application runtime |
| PM2             | Process manager     |
| Nginx           | Reverse proxy       |
| SSL (Certbot)   | HTTPS               |
| MongoDB / Redis | Database / Cache    |

---

## 🔹 11. Reference Setup Guides

* **Nginx + Node.js**
  [https://github.com/cseswapon/Nginx_SETUP_SIMPLE_DOC/blob/main/NGINX_NODE_SETUP.md](https://github.com/cseswapon/Nginx_SETUP_SIMPLE_DOC/blob/main/NGINX_NODE_SETUP.md)

* **Nginx + SSL (Certbot)**
  [https://github.com/cseswapon/Nginx_SETUP_SIMPLE_DOC/blob/main/NGINX_NODE_SSL_CERTBOT.md](https://github.com/cseswapon/Nginx_SETUP_SIMPLE_DOC/blob/main/NGINX_NODE_SSL_CERTBOT.md)

* **Nginx Troubleshooting**
  [https://github.com/cseswapon/Nginx_SETUP_SIMPLE_DOC/blob/main/NGINX_NODE_TROUBLESHOOTING.md](https://github.com/cseswapon/Nginx_SETUP_SIMPLE_DOC/blob/main/NGINX_NODE_TROUBLESHOOTING.md)

* **Redis Setup**
  [https://github.com/cseswapon/Nginx_SETUP_SIMPLE_DOC/blob/main/REDIS_SETUP.md](https://github.com/cseswapon/Nginx_SETUP_SIMPLE_DOC/blob/main/REDIS_SETUP.md)

* **MongoDB Setup**
  [https://github.com/cseswapon/Nginx_SETUP_SIMPLE_DOC/blob/main/VPS_MONGODB_SETUP.md](https://github.com/cseswapon/Nginx_SETUP_SIMPLE_DOC/blob/main/VPS_MONGODB_SETUP.md)


* **GitHub SSH Setup**

[GitHub Setup](https://github.com/cseswapon/github-multi-ssh-setup)